# Overview

glide is a library for android that makes it easier to create gifs using image views. You can even use a url link!

# java 
Keep in mind the Glide.with method uses a context object as it's parameter


> Glide.with(this).load("https://d205bpvrqc9yn1.cloudfront.net/0001.gif").into((ImageView) findViewById(R.id.gif_view));

# xml
you literally just call an imageview in the java.

# dependency

in the module gradle dependency do this.
>implementation 'com.github.bumptech.glide:glide:4.11.0'
> annotationProcessor 'com.github.bumptech.glide:compiler:4.11.0'
