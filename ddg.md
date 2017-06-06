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

"How is the feed initialized?" <<<<<<<<<<<<<<<<<<<<<<<<<<<<<<<< TODO: START HERE
