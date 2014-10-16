---
layout: post
title: Penguin Hunter UI Update
excerpt: "Playing around in Unity with the new UI panel component."
tags: [penguin-hunter, gamedev, UI, Unity]
comments: true
image:
  feature: 2hs.png
  credit: LucasArts
---
## UI Ideas

Here the 'TAB' key hides and unhides a UI Panel.  

![Left Side Slideout UI]({{ site.url }}/images/201410/slide_out_ui.gif)

---
I put together a little animation finite state machine with some simple tweening to make it a little less boring.

![UI FSM]({{site.url}}/images/201410/ui_state_machine.png)

---

Here is the simple code snippet I used to toggle the UI with the 'TAB' key.

{% highlight csharp %}
public class Slider : MonoBehaviour
{
	public Animator anim;
	public bool isHidden = true;
	public bool isActive = false;

	// Use this for initialization
	void Start ()
	{
		anim = GetComponent<Animator> ();
	}
	
	// Update is called once per frame
	void Update ()
	{
		if (Input.GetKeyDown (KeyCode.Tab)) {
			if (!isActive) {
				isActive = true;
				anim.SetTrigger ("activateUI");
			}
			isHidden = !isHidden;
			anim.SetBool ("isHidden", isHidden);
		}
	}
}
{% endhighlight %}
