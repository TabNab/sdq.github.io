---
layout:     post
title:      "JazzHands使用心得"
date:       2015-05-08 12:00:00
author:     "sdq"
header-img: "img/post-bg-06.jpg"
---

<h2 class="section-heading">JazzHands使用心得</h2>

<p>JazzHands是一个基于关键帧的动画框架，尤其适合制作Scrollview引导动画，由IFTTT开发完成。</p>

<p>核心分为两个步骤：</p>
1. 将需要显示的view元素放置在Scrollview上。
2. 针对各个view元素实现动画效果。

<p>JazzHands可支持的动画类型如下：</p>
* **IFTTTAlphaAnimation** animates the alpha property (creates fade effects).
* **IFTTTAngleAnimation** animates a rotation transform (for rotation effects).
* **IFTTTColorAnimation** animates the backgroundColor property.
* **IFTTTConstraintsAnimation** animates AutoLayout constraint constants.
* **IFTTTCornerRadiusAnimation** animates the layer.cornerRadius property.
* **IFTTTFrameAnimation** animates the frame property (moves and sizes views).
* **IFTTTHideAnimation** animates the hidden property (hides and shows views).
* **IFTTTScaleAnimation** applies a scaling transform (to scale view sizes).
* **IFTTTTransform3DAnimation** animates the layer.transform property (for 3D transforms).

<p>这里重点说明几个关键动画的实现方法</p>

<h3 class="section-heading">IFTTTFrameAnimation</h3>
<p>frame animation是最核心的动画效果，在page的变化过程中将指定view从一个frame转移到另一frame，同时包含了位置与大小的变化。</p>
<p>下列代码中演示的是在ScrollView从page1到page2的过程中，对myVoice这个UIViewImage设置frame动画。其中，最后两行针对两个关键帧设置了view在此关键帧的位置，JazzHands会完成中间的过渡。</p>
<pre><code>IFTTTFrameAnimation *myVoiceFrameAnimation = [IFTTTFrameAnimation new];
myVoiceFrameAnimation.view = self.myVoice;
[self.animator addAnimation:myVoiceFrameAnimation];
[myVoiceFrameAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(1)                                                   andFrame:self.myVoice.frame]];
[myVoiceFrameAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(2) andFrame:CGRectOffset(self.myVoice.frame, timeForPage(3), 0)]];
</code></pre>

<h3 class="section-heading"> IFTTTAlphaAnimation </h3>
<p>第二个比较实用的是透明度动画，因为在实际运用中经常会需要淡入淡出之类的效果。</p>
<p>也是同样的原理，在关键帧设置view的透明度，下面的示例代码中在page1设置为不透明，在page2中设置为完全透明，所产生的效果就是在从第一页滑动到第二页时，view将产生淡出的动画。
<pre><code>[titleView1AlphaAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(1) andAlpha:1.0f]];
[titleView1AlphaAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(2) andAlpha:0.0f]];
</code></pre>

<h3 class="section-heading"> 混合动画 </h3>
<p>当在动画中需要产生这样的效果：view1（苹果）逐渐变化为view2（香蕉）。这个时候就可以将IFTTTFrameAnimation与IFTTTAlphaAnimation混合使用。</p>
<p>具法做法的程序如下，例子中通过frame与alpha动画的混合使用，产生redPoint与pdfIcon两个UIImageView变换的动画效果：</p>
<pre><code>[redPointFrameAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(1) andFrame:self.redPoint.frame]];
[redPointFrameAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(2) andFrame:self.pdfIcon.frame]];
[redPointAlphaAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(1) andAlpha:1.0f]];
[redPointAlphaAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(2) andAlpha:0.0f]];
[pdfIconFrameAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(1) andFrame:self.redPoint.frame]];
[pdfIconFrameAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(2) andFrame:self.pdfIcon.frame]];
[pdfIconAlphaAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(1) andAlpha:0.0f]];
[pdfIconAlphaAnimation addKeyFrame:[[IFTTTAnimationKeyFrame alloc] initWithTime:timeForPage(2) andAlpha:1.0f]];
</code></pre>

<h3 class="section-heading"> StyledPageControl </h3>
<p>引导动画中常常会用到pageControl，但是如果采用官方的pageControl不免有些单调，这里推荐使用StyledPageControl，可以很简单地对pageControl进行设置。</p>
<p>这里要注意的是：需要将pageControl加载到self.view上，而不是self.scrollView</p>

<h4 class="section-heading"> 项目代码 </h4>
<p>示例代码是我在<a href="http://talk.ai">简聊2.0</a>开发过程中制作的引导动画，完整代码请戳<a href="https://github.com/sdq/Intro-Guide-View-for-Talk.ai">链接</a></p>
