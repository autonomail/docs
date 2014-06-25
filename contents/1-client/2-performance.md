Email is something we use everyday. It needs to be seamless and straightforward to use. And most of all, it needs to be quick because we've all got other things to do.

We've put a lot of effort into making our app feel responsive and slick:

* **Download once, use forever.** You download our app once to your device and then run it locally when you want to use it. Unlike most webmail, this means all the code needed to run our app is already on your computer and does not need to be fetched again. It is also better from a security point of view.

* **Fast email protocol.** Traditional email protocols were built for at time when internet connections had less bandwidth and a lot more more lag. We use a modern web-based protocol for communicating with our server, saving you time and enabling you to get to your emails quicker.

* **Do one thing well** - Many other email clients try to incorporate additional features such as calendaring, to-do lists, chat boxes, etc. We focus one doing one thing and one thing well - email.

---

### Technical details

Our email protocol is essentially a REST protocol operating over a HTTP+TLS channel. Our app UI is built with Twitter Bootstrap and Angular.JS