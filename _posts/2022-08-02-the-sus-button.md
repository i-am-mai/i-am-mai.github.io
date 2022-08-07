---
title: "A first Android app: The sus button"
toc: true
toc_sticky: true
excerpt: What could be better for a plunge into Android development than a simple family gag? Or so I thought...
header:
    overlay_image: /assets/images/posts/the-sus-button/header.webp
    overlay_filter: linear-gradient(rgba(0, 0, 0, 0), rgba(255, 0, 0, 0.3))
short: the-sus-button
---
# An idea is born
While on a brief shopping trip at the mall with my family, we had a running gag of playing the following sound effect when anyone made a slightly 'sus' joke.

{% include video id="ekL881PJMjI" provider="youtube" %}

And while it was funny when everything aligned perfectly, it was kind of hard to get the timing of the sound effect correct. Thus, the brilliant, totally original idea was born &mdash; **the Sus Button**.

# The Sus Button
> The sus button is an all-new groundbreaking take on soundboards, with the innovative 'sus'-factor that all other market-leading brands lack.

Basically, I just wanted to make a button that would play this sound. Simple enough, right?

# Development
Unfortunately, after doing a tutorial or two, I still had basically no understanding of Android app architecture (and still don't, to this day). However, what I did learn was the bare minimum to make this sus app work properly.

And so, after hours of scouring Stack Overflow for how to make an pressing an ImageButton start a MediaPlayer, I finally came out with *something* that works:

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    final MediaPlayer mp = MediaPlayer.create(this, R.raw.among_us);
    final MediaPlayer mpDeep = MediaPlayer.create(this, R.raw.among_us_deep);

    ImageButton report = (ImageButton) this.findViewById(R.id.imageButton3);
    report.setOnClickListener(new View.OnClickListener() {
        public void onClick(View v) {
            mp.start();
        }
    });

    ImageButton reportDeep = (ImageButton) this.findViewById(R.id.imageButton);
    reportDeep.setOnClickListener(new View.OnClickListener() {
        public void onClick(View v) {
            mpDeep.start();
        }
    });
}
```

Which (I think) basically just creates a `MediaPlayer` for each audio sample I wanted to use, then adds an `OnClickListener` to each button so that each respective `MediaPlayer` plays when its button is pushed. 

Now, it's probably better to just use a single `MediaPlayer` and have it switch the audio it plays, since that would probably use less resources, but at this point all I cared about was that it worked.

# The final product
{% include picture.html img="1_sm" ext="png" alt="Two buttons labeled 'Report'." caption="The Sus Buttons" %}
Behold, the sus butons. The bottom button plays the audio that I linked above. The top, however, plays this audio:

{% include video id="Regpv0xU3ZQ" provider="youtube" %}