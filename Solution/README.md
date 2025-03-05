# Puzzle-69 - A QuickGrid of All Sorts
The Blazor QuickGrid let's you set up tabular data for presentation, and it also has a sorting feature.  You'll want to review this puzzle to make sure you're getting the most from your QuickGrid sort experience

YouTube Video: https://youtu.be/

Blazor Puzzle Home Page: https://blazorpuzzle.com

## The Challenge

We've got a grid of some of our first episodes here and we'd like to add some sorting to it.

![A grid of Blazor Puzzle episodes](Images/grid.png)

The title and episode number columns already have a sort on them and they work great sorting by strings and by numbers.  What about the published date column?  How do we add sorting capabilities to that column so that it sorts dates properly?

## The Solution

The PropertyColumn in the grid contains a `Property` attribute with the formatted format for the date:

```xml
<QuickGrid Items="@TheList" >
	<PropertyColumn Property="@(e => e.Title)" Title="Title" Sortable="true" />
	<PropertyColumn Property="@(e => e.Number)" Title="Episode #" Sortable="true"/>
	<PropertyColumn Property="@(e => e.PublishDate.ToString("d"))" Title="Publish Date" />
...
</QuickGrid>
```

With QuickGrid, the `Property` should contain just the information needed to get the data to be presented.  We can add an extra attribute to declare the format to be applied, and in fact it uses the `ToString` method under the covers.  Combined with the addition of the `Sortable="true"` attribute and we've got our working sortable date column

```xml
<QuickGrid Items="@TheList" >
	<PropertyColumn Property="@(e => e.Title)" Title="Title" Sortable="true" />
	<PropertyColumn Property="@(e => e.Number)" Title="Episode #" Sortable="true"/>
	<PropertyColumn Property="@(e => e.PublishDate" Format="d" Title="Publish Date" Sortable="true" />
...
</QuickGrid>
```

## BONUS PUZZLE!

That's great, we can sort the dates... but what about the IMAGE column?  That's an image, can you sort that column too?

## The bonus solution

Yes, you can add sorting to any column.  In the case of a template column like the image, you can provide a delegate called `GridSort` that defines how to sort that column.

```xml
<QuickGrid Items="@TheList" >
...
	<TemplateColumn Title="Image" SortBy="EpisodeSort">
		<ChildContent>
			<img height="50" src="..."/>
		</ChildContent>
	</TemplateColumn>
	...
</QuickGrid>
```

```csharp
@code {

	GridSort<Episode> EpisodeSort = GridSort<Episode>
		.ByDescending(a => a.Title);

}
```