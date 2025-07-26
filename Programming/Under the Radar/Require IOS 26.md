#UnderTheRadar
### Highlights

- **The Annual Dilemma:** The hosts discuss the yearly question for iOS developers: <mark style="background: #ADCCFFA6;">whether to drop support for the previous iOS version</mark> (in this case, iOS 18) and require the new one (iOS 26) right away.
    
- **Developer vs. Business Needs:** The conversation centers on the conflict between the developer's desire for a simpler, <mark style="background: #BBFABBA6;">more productive workflow by only supporting the latest iOS</mark>, and the business need to avoid alienating existing users and <mark style="background: #FF5582A6;">cutting off potential new customers on older systems.</mark>
    
- **Data-Driven Decisions:** David Smith emphasizes using data to inform this decision, citing his own app, Widgetsmith. He notes that even a year after a new iOS release, <mark style="background: #ADCCFFA6;">a small but significant percentage of new downloads still come from users on the older OS</mark>.
    
- **A "Crazy" but Effective Solution:** David proposes a unique strategy: creating a parallel, <mark style="background: #D2B3FFA6;">duplicated version of the app's view hierarchy</mark>. This allows developers to build a "pure" iOS 26 experience without breaking compatibility for users on iOS 18, as the app essentially runs two different versions of the UI based on the user's operating system.
    
- **Sponsorship:** The episode is sponsored by Sentry, an error monitoring and tracing platform, which is presented as a tool to help developers spend less time on debugging and more on building features.
### Insights

- **Adoption Timelines:** David provides an insightful timeline for new iOS adoption. He notes that the broad, automatic update rollout typically doesn't happen until late November, coinciding with the period after new iPhones have launched and initial bugs are fixed. By December, a significant majority (around 70% for Widgetsmith) of new downloads come from the latest OS.
    
- **The "All or Nothing" Nature of Support:** The hosts point out that the <mark style="background: #FFB86CA6;">effort to support one older version (N-1) is often nearly the same as supporting multiple older versions (N-2, N-3). </mark>This suggests that developers either go all-in on the new OS or maintain broader backward compatibility.
    
- **Impact on Design:** Marco raises the concern that maintaining backward compatibility might subconsciously hold back a developer from doing a more aggressive and "pure" redesign for the new OS.
    
- **Benefits of a Day One Release:** Releasing an updated app on the first day of a new iOS launch can lead to <mark style="background: #FFB8EBA6;">press and attention from Apple</mark>. However, it also means competing with many other apps for that attention and launching to the smallest possible audience of early adopters.
### Concerns

- **Losing New Users:** The primary business concern is that by requiring the latest iOS, an app becomes unavailable to new users who have not yet updated. Since acquiring new users is challenging, proactively cutting off a portion of that market is a difficult decision.
    
- **Existing User Experience:** While existing users on older OS versions can keep the last compatible version of an app, they will not receive future bug fixes. This can leave them with a buggy experience if a problem is discovered after support is dropped.
    
- **Technical Debt and Awkwardness:** Supporting older iOS versions, especially in SwiftUI, is described as "awkward" and "cumbersome." The way modifiers are chained in SwiftUI makes it difficult to have conditional code for different OS versions within the same view structure.
    
- **Testing and QA Complexity:** Supporting multiple OS versions increases the testing burden. Developers need to perform deep testing on older systems to ensure that new features or changes for the latest OS don't inadvertently break something for users on previous versions.