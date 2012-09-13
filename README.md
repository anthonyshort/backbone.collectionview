# backbone.collectionview

A view optimized for rendering collections. Give the CollectionView a collection and a view to render each item with
and it will take care of updating the view with the collection.

## Getting Started

Download the [production version][min] or the [development version][max].

[min]: https://raw.github.com/anthonyshort/backbone.collectionview/master/dist/backbone.collectionview.min.js
[max]: https://raw.github.com/anthonyshort/backbone.collectionview/master/dist/backbone.collectionview.js

In your web page:

```html
<script src="dist/backbone.collectionview.min.js"></script>
```

Create a CollectionView and pass it the collection and the item view.

```js
// Our collection of tasks
var tasks = new Backbone.Collection();

// Create a view for a task list item
var TaskListItemView = Backbone.View.extend({
  template: 'templates/task'
});

// Create a collection view. Tell it which view to
// use for rendering the items. Alternatively this can be passed
// into the options when creating a collectionview instance.
var TaskListView = Backbone.CollectionView.extend({
  itemView: taskListItemView
}); 

// Now create an instance of the CollectionView
var todolist = new TaskListView({
  collection: tasks
});

// Render it on the page
todolist.render();
todolist.renderAllItems();

// Add an item to the list and the collection
// Listens for the 'add' event and creates a view
tasks.create({ 
  title: 'Pick up milk'
});

// Shortcut to directly create a model in the collection
// via the collection view. 
todolist.add({
  title: 'Eat some bacon'
});
```

## Documentation
_(Coming soon)_

## Examples
_(Coming soon)_

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [grunt](https://github.com/cowboy/grunt).

_Also, please don't edit files in the "dist" subdirectory as they are generated via grunt. You'll find source code in the "src" subdirectory!_

## Release History
0.1.0 - First release

## License
Copyright (c) 2012 Anthony Short  
Licensed under the MIT license.
