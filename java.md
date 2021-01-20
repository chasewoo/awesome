## How to install Java JDK on Mac

```
$ brew tap AdoptOpenJDK/openjdk
$ brew install --cask adoptopenjdk8

# Check if it is completed
$ java -version

# Set the JAVA_HOME
/usr/libexec/java_home -V
#/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
vim ~/.zshrc # if you use zsh
# add the new lint to bottom of .zshrc.
# export JAVA_HOME="/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home"
```
