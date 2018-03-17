## About

[![Build Status](https://img.shields.io/circleci/project/0xced/XCDYouTubeKit/master.svg?style=flat)](https://circleci.com/gh/0xced/XCDYouTubeKit)
[![Coverage Status](https://img.shields.io/codecov/c/github/0xced/XCDYouTubeKit/master.svg?style=flat)](https://codecov.io/gh/0xced/XCDYouTubeKit/branch/master)
[![Platform](https://img.shields.io/cocoapods/p/XCDYouTubeKit.svg?style=flat)](http://cocoadocs.org/docsets/XCDYouTubeKit/)
[![Pod Version](https://img.shields.io/cocoapods/v/XCDYouTubeKit.svg?style=flat)](https://cocoapods.org/pods/XCDYouTubeKit)
[![Carthage Compatibility](https://img.shields.io/badge/Carthage-compatible-4BC51D.svg?style=flat)](https://github.com/Carthage/Carthage/)
[![License](https://img.shields.io/cocoapods/l/XCDYouTubeKit.svg?style=flat)](LICENSE)

**XCDYouTubeKit** is a YouTube video player for iOS, tvOS and macOS.

## Note from Sn0wCh1ld
Honestly, I wouldn't use this if I were you. The API this library is based on has been deprecated since iOS 9. Before using this, please check the [original repo](https://github.com/0xced/XCDYouTubeKit) to see if it has been updated to use the more modern AVPlayerViewController rather than MPMoviePlayerViewController (or something even newer that hasn't been released as of the writing of this note.

As of this writing (March 17th, 2018), the only difference between this fork and the original library is that this one allows you to load videos via NSURL (including local files), rather than needlessly restricting it to YouTube.

## Requirements

- Runs on iOS 8.0 and later
- Runs on macOS 10.9 and later
- Runs on tvOS 9.0 and later

## Warning

XCDYouTubeKit is against the YouTube [Terms of Service](https://www.youtube.com/t/terms). The only *official* way of playing a YouTube video inside an app is with a web view and the [iframe player API](https://developers.google.com/youtube/iframe_api_reference). Unfortunately, this is very slow and quite ugly, so I wrote this player to give users a better viewing experience.

## Installation

This fork of XCDYouTubeKit is available through CocoaPods.

CocoaPods:

```ruby
pod 'XCDYouTubeKit', :git => 'https://github.com/Sn0wCh1ld/XCDYouTubeKit.git'
```

Alternatively, you can manually use the provided static library or dynamic framework. In order to use the static library, you must:

1. Create a workspace (File → New → Workspace…)
2. Add your project to the workspace
3. Add the XCDYouTubeKit project to the workspace
4. Drag and drop the `libXCDYouTubeKit.a` file referenced from XCDYouTubeKit → Products → libXCDYouTubeKit.a into the *Link Binary With Libraries* build phase of your app’s target.

These steps will ensure that `#import <XCDYouTubeKit/XCDYouTubeKit.h>` will work properly in your project.

## Usage

XCDYouTubeKit is [fully documented](http://cocoadocs.org/docsets/XCDYouTubeKit/).

### iOS only

On iOS, you can use the class `XCDYouTubeVideoPlayerViewController` the same way you use a `MPMoviePlayerViewController`, except you initialize it with a YouTube video identifier instead of a content URL.

#### Present the video in full-screen

```objc
- (void) playVideo
{
	XCDYouTubeVideoPlayerViewController *videoPlayerViewController = [[XCDYouTubeVideoPlayerViewController alloc] initWithVideoIdentifier:@"9bZkp7q19f0"];
	[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(moviePlayerPlaybackDidFinish:) name:MPMoviePlayerPlaybackDidFinishNotification object:videoPlayerViewController.moviePlayer];
	[self presentMoviePlayerViewControllerAnimated:videoPlayerViewController];
}

- (void) moviePlayerPlaybackDidFinish:(NSNotification *)notification
{
	[[NSNotificationCenter defaultCenter] removeObserver:self name:MPMoviePlayerPlaybackDidFinishNotification object:notification.object];
	MPMovieFinishReason finishReason = [notification.userInfo[MPMoviePlayerPlaybackDidFinishReasonUserInfoKey] integerValue];
	if (finishReason == MPMovieFinishReasonPlaybackError)
	{
		NSError *error = notification.userInfo[XCDMoviePlayerPlaybackDidFinishErrorUserInfoKey];
		// Handle error
	}
}

```

#### Present the video in a non full-screen view

```objc
XCDYouTubeVideoPlayerViewController *videoPlayerViewController = [[XCDYouTubeVideoPlayerViewController alloc] initWithVideoIdentifier:@"9bZkp7q19f0"];
[videoPlayerViewController presentInView:self.videoContainerView];
[videoPlayerViewController.moviePlayer play];
```

### iOS, tvOS and macOS

```objc
NSString *videoIdentifier = @"9bZkp7q19f0"; // A 11 characters YouTube video identifier
[[XCDYouTubeClient defaultClient] getVideoWithIdentifier:videoIdentifier completionHandler:^(XCDYouTubeVideo *video, NSError *error) {
	if (video)
	{
		// Do something with the `video` object
	}
	else
	{
		// Handle error
	}
}];
```

See the demo project for more sample code.

## Logging

Since version 2.2.0, XCDYouTubeKit produces logs. XCDYouTubeKit supports [CocoaLumberjack](https://github.com/CocoaLumberjack/CocoaLumberjack) but does not require it.

See the `XCDYouTubeLogger` class [documentation](http://cocoadocs.org/docsets/XCDYouTubeKit/) for more information.

## Credits

The URL extraction algorithms in *XCDYouTubeKit* are inspired by the [YouTube extractor](https://github.com/rg3/youtube-dl/blob/master/youtube_dl/extractor/youtube.py) module of the *youtube-dl* project.

## Contact

Cédric Luthi

- http://github.com/0xced
- http://twitter.com/0xced

## License

XCDYouTubeKit is available under the MIT license. See the [LICENSE](LICENSE) file for more information.
