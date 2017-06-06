# Outline

## DuckDuckGo mobile app

<!----------------------------------------------------------------------------------------------------->


#### tmp

**Capabilities**

- Autocomplete (`DDGAutoCompleteTextView`)


**Structure**

"Where's the main startup sequence?" (**NOTE: structure each subsection of talk as a series of questions, always starting with "Where's the main startup sequence?"**)

- `activity.DuckDuckGo` [is the main activity](https://sourcegraph.com/github.com/duckduckgo/android-search-and-stories/-/blob/src/com/duckduckgo/mobile/android/activity/DuckDuckGo.java#L118:21)
  - `FragmentManager`
  - `Toolbar`
  - `DDGAutoCompleteTextView`
  - `SharedPreferences` ??
  - `OnboardingHelper`: simple class for managing onboarding state (whether or not to onboard, simple actions that comprise onboarding)
- Notably missing: use of Butterknife, Dagger, or any other dependency injection tools
- `ImageCache`: downloading images efficiently (how does Wikipedia do this? Some reinvention of the wheel between `ImageCache` and Wikipedia's `WikiCachedService`)

"How is the feed initialized?"

- `DuckDuckGo.onCreate` -> `displayHomeScreen` -> `displayScreen` -> `changeFragment`
  - `FragmentManager.beginTransaction` + `FragmentTransaction`, `FragmentManager.popBackStack` + `FragmentManager.popBackStackImmediate`

- [`DDGControlVar`](https://sourcegraph.com/github.com/duckduckgo/android-search-and-stories/-/blob/src/com/duckduckgo/mobile/android/util/DDGControlVar.java#L26:48) an interesting way to keep track of global state

- [`SCREEN`](https://sourcegraph.com/github.com/duckduckgo/android-search-and-stories/-/blob/src/com/duckduckgo/mobile/android/util/SCREEN.java#L3:13) enum: a different approach to reverse mapping an enum type (contrast with the map-based approach in the Wikipedia app)

"How does one issue a search query?"

"How does autocomplete work?"

"How are results fetched?"
