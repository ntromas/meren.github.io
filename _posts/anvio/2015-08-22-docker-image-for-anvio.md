---
layout: post
title: "Docker image for anvi'o"
excerpt: "Fresh anvi'o builds for the lazy."
modified: 2015-08-22
tags: []
categories: [anvio]
comments: true
authors: [meren]
---

{% include _project-anvio-version.html %}

{% include _toc.html %}

If you are reading this, you probably already know what [Docker](https://www.docker.com/https://www.docker.com/) is. If you don't know, here is a quote from their web site:

<blockquote>
An open platform for distributed applications for developers and sysadmins.
</blockquote>

If you do not want to deal with the [oldschool installation manual]({% post_url anvio/2016-06-26-installation-v2 %}), you can try anvi'o using its Docker image (special thanks go to to my good friend [Çağlar Onur](https://twitter.com/caglar10ur) for forcing us to [have it](https://github.com/meren/anvio/pull/191)). We now maintain [anvi'o docker images online](https://hub.docker.com/r/meren/anvio/tags/) as closely as possible to the latest stable release.

If you want to quickly run anvi'o on a server or a desktop system with 0 pain, this could be a good solution for you. First chapter is for Linux users. The next chapter is going to explain how you can run anvi'o in a couple of minutes on your OSX or Windows installation (I'd like to thank [Cameron MacPherson](https://www.linkedin.com/pub/cameron-macpherson/3/279/219) for [suggesting](https://github.com/meren/anvio/issues/213#issuecomment-148481913) this).

## Running the anvi'o docker image (for Linux users)

I assume the server you are working on has Docker installed, and the docker service is up and running. I run these commands on my Ubuntu Linux box to install Docker, and running it: 

{% highlight bash %}
$ sudo apt-get install docker.io
$ sudo service docker.io start
{% endhighlight %}

Now you can simply type this to get all the anvi'o tags available to you:

{% highlight bash %}
$ sudo docker pull meren/anvio
{% endhighlight %}

Here are the available tags as of today:

{% highlight bash %}
$ sudo docker images
REPOSITORY          TAG        IMAGE ID     CREATED    SIZE
meren/anvio         2.0.2      (...)        (...)      672.0 MB
meren/anvio         2.0.1      (...)        (...)      671.9 MB
meren/anvio         1.2.3      (...)        (...)      595.2 MB
{% endhighlight %}

{:.notice}
Although we will do our best to keep this post up-to-date, please remember that there may be a newer docker image at any given time. In that case, please use the most recent docker image and make sure the user tutorial matches to the version you are using.

Now you can simply type this to get the latest version running (or you can replace `latest` with the version number you see in the list of TAGs):

{% highlight bash %}
$ sudo docker run --rm -it meren/anvio:latest
{% endhighlight %}

This will give you a new prompt in your terminal that will look like this:

{% highlight bash %}
:: anvi`o ::  / >>>
{% endhighlight %}

Once you have the anvi'o prompt, you can run a small test:

{% highlight bash %}
:: anvi`o ::  / >>> anvi-self-test 
{% endhighlight %}


At the end of the test you can press `CTRL`+`C` to kill the server, and press `CTRL`+`D` to go back to your host terminal.


## Running the anvi'o docker image (for Mac OSX / Windows users)

First, you will need to install the [Docker Toolbox](https://www.docker.com/toolbox). Once you have it installed, run 'Docker Quickstart Terminal' application. At the end, this is what you want to see:

<div class="centerimg">
<a href="{{ site.url }}/images/anvio/2015-08-22-docker-image-for-anvio/docker-terminal.png"><img src="{{ site.url }}/images/anvio/2015-08-22-docker-image-for-anvio/docker-terminal.png" /></a>
</div>

Note the IP address (which is `192.168.99.100` in my case), becasue you will need it later. At this point, all you need to do is to run this command (note that you may need to change the version number):

{% highlight bash %}
docker run -p 8080:8080 -it meren/anvio:latest
{% endhighlight %}

Docker will download all the necessary images, and will finally give you an anvi'o prompt which will look like this:

{% highlight bash %}
:: anvi`o ::  / >>>
{% endhighlight %}

Once you see this, you can run the self-test:

{% highlight bash %}
:: anvi`o ::  / >>> anvi-self-test 
{% endhighlight %}

Now you can open your Chrome browser, type the IP address you noted with a ":8080" at the end (which looks like this for me 192.168.99.100:8080), and press enter... Surprise!


## How about my files?

You will soon realize that this new virtual environment in which you run anvi'o does not have access to the rest of your disk.

Let's assume the data you would like to analyze using anvi'o is in a directory at `/home/meren/my_data`. To have access to this directory from within the docker image, you can _bind_ it to a directory in the virtual environment. Let's assume, we want to have access to the content of `/home/meren/my_data`, from `/my_data` directory from within the docker image:

{% highlight bash %}
$ sudo docker run --rm -v /home/merenbey/my_data:/my_data -it meren/anvio:latest
{% endhighlight %}

That's it. Here is a longer story:

{% highlight bash %}
$ pwd
/home/meren
$ ls my_data/
X.bam  Y.bam  contigs.fa
$ sudo docker run --rm -v /home/merenbey/my_data:/my_data -it meren/anvio:latest                                      
:: anvi`o ::  / >>> ls /my_data/
X.bam  Y.bam  contigs.fa
{% endhighlight %}


## Rebuilding the anvi'o docker image (for hackers)

If you would like to rebuild the Docker image for anvi'o on your own server, you can simply clone the anvi'o repository:

{% highlight bash %}
$ git clone --recursive https://github.com/meren/anvio.git
{% endhighlight %}

Switch to a particular tag (here is [a complete list of available releases and tags](https://github.com/meren/anvio/releases)):

{% highlight bash %}
git checkout tags/v2.0.2
{% endhighlight %}

Add/remove things you want, do your changes in the code, and build the new docker image (replace the username and tag with your preferences):

{% highlight bash %}
docker build -t meren/anvio:2.0.2.
{% endhighlight %}

And optionally, push it to your account on the hub to allow other people run it easily (i.e., this is what I do to push the new images to my account):

{% highlight bash %}
docker push meren/anvio
{% endhighlight %}

Don't hesitate to get in touch if you have any questions, and check out the [anvi'o project page]({{ site.url }}/software/anvio).
