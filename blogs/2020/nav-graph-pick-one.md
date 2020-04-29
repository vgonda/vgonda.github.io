[Blogs](../../blogs.md) ❖ [Books](../../books.md) ❖ [Talks](../../talks.md) ❖ [Newsletter](https://tinyletter.com/vgonda) ❖ [Twitter](https://twitter.com/TTGonda)

# Pick one: Setting your nav graph

**tl;dr** Only add your nav graph one place. Want to know why, read on.

I was learning how to use navigation components and I did what many of us do: read a bunch of blog posts to piece together how to do what I wanted to accomplish. You all have created so many great resources on how to use the latest libraries. I had navigation set up for this feature in no time.

It wasn't long after that I started to notice some odd behavior. When rotating the device, I would always end on my start destination no matter where I was in the graph. For example, say I had a start destination fragment of a list and a detail fragment that I navigated to when you click a list item. After navigating to the detail fragment, then rotate the device, I would be back on the start destination list.

I dug into the source code (which I later discovered I misunderstood). It suggested to me that I should only set the graph in the activity if it was the first time the activity was opened. I added a check for `savedInstanceState`, and it seemed to solved the problem. I soon found out I was mistaken.

```kotlin
// DON'T DO THIS
if (savedInstanceState == null) {
    findNavController(R.id.navHostFragment)
        .setGraph(R.navigation.nav_graph)
}
// DON'T DO THIS
```

I got into a conversation about another problem I was trying to solve, and this came up. It was there I learned this gotcha: **you shouldn't set your nav graph in both your layout xml file and the activity code**. This essentially sets the graph twice and resets everything.

You can **either** set it in your xml using `app:navGraph`:

```xml
<fragment
    android:id="@+id/navHostFragment"
    android:name="androidx.navigation.fragment.NavHostFragment"
    app:defaultNavHost="true"
    app:navGraph="@navigation/new_events_nav_graph" />
```

**OR** you can set it in your activity using `setGraph`:

```kotlin
findNavController(R.id.navHostFragment)
    .setGraph(R.navigation.nav_graph)
```

Moral of the story: Pick on place to set your graph.

Huge thanks to [Ian Lake](https://twitter.com/ianhlake) for pointing this out to me.

_04/06/2020_

-----

**Did you know I co-authored a book about testing on Android?** [Check it out](../../books.md)

---

If you like my work, consider [buying me a coffee ☕](https://www.buymeacoffee.com/96JjLEW)!
