---
layout: post
title: Updating UI from the backend
excerpt: "Connecting Unity to Parse.com to establish a database connection, and update the UI."
tags: [penguin-hunter, gamedev, UI, Unity]
comments: true
image:
  feature: 2hs.png
  credit: LucasArts
---
Here I use Unity to authenticate with Parse.com (a backend provider) to give access to the project data, including which animals exist in the game, and how many of those creatures are remaining.  Since getting this information relys on an API call which takes an unknown amount of time, this task is asynchronous.  After some digging around (and the help of the Unity Answers community) I found this nice example in the Parse documentation.

{% highlight csharp %}
public IEnumerator GameOver()
{
    var gameHistory = new ParseObject("GameHistory");
    gameHistory["score"] = score;
    gameHistory["player"] = ParseUser.CurrentUser;
 
    var saveTask = gameHistory.SaveAsync();
    while (!saveTask.IsCompleted) yield return null;
 
    // When the coroutine reaches this point, the save will be complete
 
    var historyQuery = new ParseQuery<ParseObject>("GameHistory")
        .WhereEqualTo("player", ParseUser.CurrentUser)
        .OrderByDescending("createdAt");
 
    var queryTask = historyQuery.FindAsync();
    while (!queryTask.IsCompleted) yield return null;
 
    // The task is complete, so we can simply check for its result to get
    // the current player's game history
    var history = queryTask.Result;
}
{% endhighlight %} 

Used this way I setup a coroutine to fetch the game's data, and check each frame if that async task is complete. Once I have heard back from the server I continue on the main thread where I can access Unity GameObjects in the normal way.

---

This gif shows the UI being instantiated for the first time.  In the eventual implementation this will take place during the game's initialization stage so that the UI components are always there.

![Updating UI from Parse]({{ site.url }}/images/201410/slide_out_parse.gif)

---
