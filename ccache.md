# Using ccache with flutter.

If you are like me and use firebase in your flutter project then you've likely come across the issue of extremely long compile times for iOS targets. 

## The old way

One route is use the pre-compiled version. However there are often delays between when a new version of flutter comes out and when a pre-build is available for the version you need.
TK Document the old way - add link to the issue.

## The better way

Here are my notes from trying to get `ccache` working with flutter. 

1. Install `ccache` with `brew install ccache`
2. Set symlinks (with sudo)
   ```
   sudo ln -s $(which ccache) /usr/local/bin/gcc && \
   sudo ln -s $(which ccache) /usr/local/bin/g++ && \
   sudo ln -s $(which ccache) /usr/local/bin/cc && \
   sudo ln -s $(which ccache) /usr/local/bin/c++ && \
   sudo ln -s $(which ccache) /usr/local/bin/clang && \
   sudo ln -s $(which ccache) /usr/local/bin/clang++
   ```
   Note if you want to later remove the links do the following:

   ```
   sudo rm /usr/local/bin/gcc && \
   sudo rm /usr/local/bin/g++ && \
   sudo rm /usr/local/bin/cc && \
   sudo rm /usr/local/bin/c++ && \
   sudo rm /usr/local/bin/clang && \
   sudo rm /usr/local/bin/clang++
   ```
3. Make sure `which gcc` returns `/usr/local/bin/gcc`
   - If it doesn't you may need to reload your terminal session

4. Add to your podfile in the `post_install` section:
   ```
    post_install do |installer|
    installer.pods_project.targets.each do |target|
        flutter_additional_ios_build_settings(target)
        # Custom post install code can be added here
              

        target.build_configurations.each do |config|

        config.build_settings["CC"] = "clang"
        config.build_settings["LD"] = "clang"
        config.build_settings["CXX"] = "clang++"
        config.build_settings["LDPLUSPLUS"] = "clang++"
        # Using the un-qualified names means you can swap in different implementations, for example ccache
        

        config.build_settings['IPHONEOS_DEPLOYMENT_TARGET'] = '14.0'
        end
    end
    end
    
    ```
5. Build `flutter build ios`
   - First build took: 411.7s
6. run `ccache -s`

---

Links
---

https://dev.to/leehack/optimize-flutter-ios-build-using-ccache-2oi2

https://github.com/invertase/firestore-ios-sdk-frameworks

https://reactnative.dev/docs/build-speed#use-a-compiler-cache


