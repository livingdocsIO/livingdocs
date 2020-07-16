## Example of a simple bullet list

A list is basically a set of 2 components: a container component that holds the list items and a repeatable component that is inserted whenever a user presses enter in the container. You can basically make everything a repeatable. The example shows the regular use case of a bullet list.

In the container you set the `defaultComponents:paragraph` to define which component is inserted when a user presses enter (repeatable) and the `defaultContent` to define which component is initially present (template). Usually, the two are the same.

The container:
```javascript
module.exports = {
  "label": "Bullet List",
  "name": "bullet-list",
  "properties": ["list-type"],
  "directives": [{
    "name": "list",
    "type": "container",
    "defaultComponents": {
      "paragraph": "bullet-list-item"
    },
    "defaultContent": [{
      "component": "bullet-list-item"
    }]
  }],
  "html": dedent`
  <ul doc-container="list" class="ld-list"></ul>
  `
}
```

The repeatable:
```javascript
module.exports = {
  "name": "bullet-list-item",
  "label": "Bullet List Item",
  "allowedParents": ["bullet-list"],
  "html": dedent`
  <li doc-editable="text">List item</li>
  `
}
```