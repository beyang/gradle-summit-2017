# Outline

## Overall takeaways

Note: overall time: 45 minutes (leave 5-10 min for questions)

- Good artists copy. Great ones steal.
- A good way to start a new project is to take a look at existing ones like it.

THOUGHT: make second half of talk about analyzing Java and Gradle

## Brief mention

Briefly mention [android-architecture](https://github.com/googlesamples/android-architecture) repository as good source of good, simple examples. "But we are going to dive into some real-world apps" to see how things are done in the wild.
...would be good to emphasize use of third-party libraries, as well (something which the sample code will not do)

## Wikipedia mobile app

How the Wikipedia mobile app works
- Tab-based interface: "Explore", "History", "Lists", "Nearby"
- How it displays an efficient feed
- How it persists data
- How it uses the MapView

### Basic structure

- Activity
- Fragment
- Adapter
- View

### Standard library usages
- [SwipeRefreshLayout](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/feed/FeedFragment.java?q=webview#L45:47-45:65)
- [Fragment options menu customization (search icon visible)](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/feed/FeedFragment.java?q=webview#L209:17-209:37)
- [FeedView](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/feed/view/FeedView.java?q=webview#L19:14-19:22) is an example of [RecyclerView](https://sourcegraph.com/github.com/android/platform_frameworks_support@efdf532476a3639993c329de0a0d1f8c4fa53343/-/blob/v7/recyclerview/src/android/support/v7/widget/RecyclerView.java#L151:14-151:26) subclassing pattern. "Where do the cards in this feed come from?"
  - `FeedFragment`
    - `FeedView`
      - `FeedAdapter` provides new view instances, given `int viewType`, which is provided by `{FeedAdapter,RecyclerView.Adapter}.getItemViewType`
        - `FeedCoordinator`, subclasses `FeedCoordinatorBase` (where most of logic is). `FeedCoordinator.getCards` provides `Card` (model) instances. `FeedCoordinator` and `FeedCoordinatorBase` seem like they should be a single class (#techdebt?).
      - `FeedAdapter.callback`
    - `FeedFunnel`: provided by `FeedFragment` as callback to `FeedAdapter`, implements [`Funnel` interface](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/analytics/Funnel.java?q=webview#L19:28-19:34), which seems to be used for usage logging
- [WikipediaApp](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/WikipediaApp.java#L98:17) subclasses `Application`. "What kind of global state does it keep?"
  - `Database`
  - `WikiSite`
  - `apis`: map of APIs for different Wikimedia sites
  - `WikipediaApp.getDatabaseClient`
- Data persistence using SQLite
    - ReadingListFragment
    - ReadingList
        - ReadingListRow
        - ReadingListTable
    - ReadingListData singleton (lowest level DAO for database)
        - Confusing, multiple ways to reference (#techdebt?):
        - ReadingListData.instance()
        - ReadingList.DAO (Data Access Object)
        - stateless
        - note: singleton instance, rather than static methods
    - Database

### Third-party library usages
- [butterknife.BindView](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/main/MainFragment.java#L77:9)
- [ViewPager](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/main/MainFragment.java#L77:46-77:55) and [Fragment](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/main/MainFragment.java#L74:35-74:43)
- [NearbyFragment](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/nearby/NearbyFragment.java#L81:20-81:23) uses Mapbox's `MapView`
  - seems to be view <-> callbacks model
    - Register callbacks on `MapView`
    - Modify `MapboxMap` (view model)
  - Uses [NearbyClient](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/nearby/NearbyFragment.java#L88:13-88:25) to query API for nearby landmarks.
    - Uses [WikiCachedService](WikiCachedService) (subclass of CachedService, subclass of Retrofit)
      - Retrofit seems to be non-standard branch. Seems like they vendored it in at some point. Doesn't seem to be mentioned in Gradle files.

### Basic patterns
- [View initialization (MainFragment.onCreateView)](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/main/MainFragment.java#L101:37-101:49)
  - inflate View
  - bind via Butterknife
  - set tab selector delegate

### Interesting patterns
- [EnumCodeMap](https://sourcegraph.com/github.com/wikimedia/apps-android-wikipedia/app/src/main/java/org/wikipedia/main/MainFragment.java/-/blob/app/src/main/java/org/wikipedia/model/EnumCodeMap.java#L6:14): Class that converts enums into maps

### Notes (misc classes)
- ReadingListSynchronizer


<!----------------------------------------------------------------------------------------------------->


#### tmp

FeedFragment
    [x] FeedView
    [x] WikipediaApp ??
    [x] FeedCoordinator ??
    [x] FeedFunnel ??

[x] "How does it fetch data from remote?"

[x] "How does it persist data?"

PageFragment
