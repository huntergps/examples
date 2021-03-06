This example shows how you can create a neat parallax effect for pages in a `PageControl` using `EnteringAnimation`, `ExitingAnimation` and `ScrollingAnimation`.

Each page has an `EnteringAnimation` and `ExitingAnimation` pair, which is used to move the background image and other parts of the UI in the _opposite_ direction of the page.
This results in the background image moving along with the page, but slower than the page itself.
We also do the same for the `ScrollView` inside each page using `ScrollingAnimation`.
To prevent the images from overlapping, we set `ClipToBounds="true"` on each page.

```
<App>
	<iOS.StatusBarConfig Style="Light" />
	
	<Font ux:Global="RobotoThinItalic" File="assets/Roboto-ThinItalic.ttf" />
	
	<Page ux:Class="ParallaxPage">
		<string ux:Property="TitleText" />
		<float4 ux:Property="ThemeColor" />
		<FileSource ux:Property="ImageFile" />
		
		<ScrollView ux:Name="scrollView" ClipToBounds="true">
			<ScrollViewMotion Overflow="Clamp" />
			<ScrollingAnimation From="0" To="height(this)">
				<Move Target="coverBackground" Y="-80" Duration="1" />
				<Move Target="title" Y="-120" Duration="1" />
				<Change contentShadow.Color="#0007" Duration="1" />
			</ScrollingAnimation>
			
			<StackPanel>
				<Panel Height="height(this)" />
				<StackPanel Color="#eee" Margin="-1,0" ItemSpacing="10" Padding="10">
					<Shadow ux:Name="contentShadow" Size="20" Distance="10" Angle="-90" Color="#0000" />
					
					<LoremIpsum Margin="15,0" />
				</StackPanel>
			</StackPanel>
		</ScrollView>
		
		<Panel ux:Name="cover" ClipToBounds="true">
			<EnteringAnimation>
				<Move Target="title"           X="-0.2" RelativeTo="ParentSize" />
				<Move Target="swipeUpText"     X="1"    RelativeTo="ParentSize" />
				<Move Target="coverBackground" X="0.4"  RelativeTo="ParentSize" />
			</EnteringAnimation>
			<ExitingAnimation>
				<Move Target="title"           X="0.2"  RelativeTo="ParentSize" />
				<Move Target="swipeUpText"     X="-1"   RelativeTo="ParentSize" />
				<Move Target="coverBackground" X="-0.4" RelativeTo="ParentSize" />
			</ExitingAnimation>
			
			<Text ux:Name="title" Value="{ReadProperty TitleText}"
			      Alignment="Center" Color="#fff" Font="RobotoThinItalic" FontSize="60" />
			
			<Text ux:Name="swipeUpText" Alignment="BottomCenter" Color="#fff" Margin="0,0,0,25">
				Swipe up to read
			</Text>
			<WhileFalse>
				<Cycle Target="swipeUpText.Opacity" Low="0.05" High="0.4" Frequency="0.4" Waveform="Sine" />
			</WhileFalse>
			
			<Rectangle ux:Name="coverBackground">
				<Rectangle Color="#0002">
					<LinearGradient AngleDegrees="55" Opacity="0.8">
						<GradientStop Color="#0000" Offset="-0.2" />
						<GradientStop Color="{ReadProperty ThemeColor}" Offset="1.15" />
					</LinearGradient>
				</Rectangle>
				<Image Layer="Background" File="{ReadProperty ImageFile}" StretchMode="UniformToFill">
					<Desaturate Amount="0.4" />
				</Image>
			</Rectangle>
		</Panel>
	</Page>
	
	<PageControl ux:Name="pageControl" InactiveState="Unchanged">
		<ParallaxPage TitleText="TECH"     ThemeColor="#9C27B0" ImageFile="assets/tech.jpg" />
		<ParallaxPage TitleText="CULTURE"  ThemeColor="#E91E63" ImageFile="assets/culture.jpg" />
		<ParallaxPage TitleText="SPORTS"   ThemeColor="#43A047" ImageFile="assets/sports.jpg" />
		<ParallaxPage TitleText="BUSINESS" ThemeColor="#1A237E" ImageFile="assets/business.jpg" />
	</PageControl>
</App>
```
