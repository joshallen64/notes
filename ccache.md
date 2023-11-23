# Using ccache with flutter.

If you are like me and use firebase in your flutter project then you've likely come across the issue of extremely long compile times for iOS targets. 

## The old way

One route is use the pre-compiled version. However there are often delays between when a new version of flutter comes out and when a pre-build is available for the version you need.
TK Document the old way - add link to the issue.

## The better way

Here are my notes from trying to get `ccache` working with flutter. 

1. Install `ccache` with `brew install ccache`


---

Links
---

https://dev.to/leehack/optimize-flutter-ios-build-using-ccache-2oi2
