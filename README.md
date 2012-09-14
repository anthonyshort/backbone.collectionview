# Backbone.CollectionView

A view optimized for rendering collections. Give the CollectionView a collection and a view to render each item with
and it will take care of updating the view with the collection.

* Whenever a model is added or removed from a collection it will automatically be added or removed from the view. 
* It will set loading and ready states as well as showing and hiding fallback content if there are no items in the list. 
* If a model is added to a specific point in a collection it will render the item at the correct spot in the list
* It will clean up after itself so there are no memory leaks

## Getting Started

### Browser

Download the [production version][min] or the [development version][max].

[min]: https://raw.github.com/anthonyshort/backbone.collectionview/master/dist/backbone.collectionview.min.js
[max]: https://raw.github.com/anthonyshort/backbone.collectionview/master/dist/backbone.collectionview.js

```html
<script src="dist/backbone.collectionview.min.js"></script>
```

### Bower

```
bower install backbone.collectionview
```

### NPM

```
npm install backbone.collectionview
```

## Usage 

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

The first thing you need to know about creating a collection view is that it *needs* a view for the items and a collection. 

### Setting the item view

The item view is the view that is used to render each of the models in the collection. eg. In a list of tasks the item view 
is the view for a single task item. *Item views will have access to the model and the collection by default*.

You can set the item view in a few ways:

#### 1. As an instance property

```js
var TaskListView = Backbone.CollectionView.extend({
  itemView: myItemView
});
```

#### 2. In the options

```js
var TaskListView = Backbone.CollectionView.extend();
var list = new TaskListView({
  itemView: myItemView
}); 
```

You can use this to override the view set in the original definition as well. This allows you to use the same
collection view for different types of lists that have the same functionality. For example, a list of albums
in a library may have a cover view and a title view.

#### 3. Override getItemView method

If you want to do some processing on the view before it's used then you can override the `getItemView` method.

```js
var TaskListView = Backbone.CollectionView.extend({
  getItemView: function(model){
    return new myItemView({
      model: model
    }); 
  }
});
```

### Options

* `render` - Automatically render when an instance is created
* `renderItems` - Automatically render the items. This will also call `render`
* `itemView` - The view to use to render each item in the collection
* `collection` - *required* The collection whose models will be rendered.

### After Item Render Hook

There is a method named `afterRenderItem` that will be fired whenever an item is rendered in the list.

```js
afterRenderItem: function(view, model) {}
```

You can override this method if you need to do some extra processing on views after they are rendered.

### Item View Events

You can easily listen for events on the views in the collection too. Just add a `viewEvents` object to your collection view.
This is handy for things like selections.

```js
var TaskListItem = Backbone.View.extend({
  events: {
    'click': 'select'
  },
  select: function(event) {
    this.trigger('select', this);
  }
});

var TaskListView = Backbone.CollectionView.extend({
  itemView: TaskListItem,
  viewEvents: {
    'select': 'onItemSelect'
  },
  initialize: function(options) {
    this.selected = [];
  },
  onItemSelect: function(view) {
    this.selected.push(view);
  }
});
```

### Collection Events

You can also listen for events on the collection easilt as well.

```js
var TaskListView = Backbone.CollectionView.extend({
  itemView: TaskListItem,
  collectionEvents: {
    'sort': 'onCollectionSort'
  },
  onCollectionSort: function(view) {
    # Do sorting stuff
  }
});
```

## Contributing
In lieu of a formal styleguide, take care to maintain the existing coding style. Add unit tests for any new or changed functionality. Lint and test your code using [grunt](https://github.com/cowboy/grunt).

_Also, please don't edit files in the "dist" subdirectory as they are generated via grunt. You'll find source code in the "src" subdirectory!_

## Release History
0.1.0 - First release

## License
Copyright (c) 2012 Anthony Short  
Licensed under the MIT license.
