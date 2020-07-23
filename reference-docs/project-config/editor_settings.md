# Editor Settings

Added in: `release-2020-07`

The editor settings are sent to the editor and control the behavior of your editor, in particular:
- user menu
- main navigation
- dashboards
- startPage
- mediaLibrary

An example:
```js
// projectConfig.editorSettings: {...}
{
  userMenu: [
    {
      label: 'What is new',
      data: {
        lastUpdate: '2020-06-16T13:19:38.560Z'
      },
      icon: 'plus',
      id: 'release_notes',
      group: 'string:required',
      // Use either 'href' (external link) or 'route' (internal link)
      href: 'https://github.com/livingdocsIO/livingdocs-release-notes',
      route: {
        name: 'app.state'
      }
    }
  ],
   mainNavigation: [
    {liItem: 'articles'},
    // liItem is a shortcut, one can also define it's own config
    {
      label: 'My Custom Articles',
      route: {
        name: 'app.articles'
      },
      icon: 'file-document',
      scope: 'readArticles',
      group: 'dashboards' // one of 'dashboards', 'preferences', 'admin'
    },
    {liItem: 'pages'},
    { // custom task dashboard
      label: 'Proofreading',
      dashboard: 'kanban-proofreading',
      icon: 'clipboard-check'
    },
    { // custom document dashboard
      label: 'Authors',
      dashboard: 'authors-dashboard',
      icon: 'account'
    },
    {liItem: 'mediaLibrary'},
    {liItem: 'lists'},
    {liItem: 'menus'},
    {liItem: 'contentSetup'},
    { // external link
      label: 'Livingdocs Website',
      icon: 'rocket',
      href: 'https://www.livingdocs.io',
      group: 'preferences'
    },
    {liItem: 'projectSettings'},
    {liItem: 'serverAdmin'}
  ],
  dashboards: [{
    handle: 'kanban-proofreading',
    type: 'taskBoard',
    pageTitle: 'Proofreading',
    taskName: 'proofreading',
    throttleTime: 2000, // ms
    displayFilters: ['documentState', 'timeRange']
  }, {
    handle: 'authors-dashboard',
    type: 'dashboard',
    pageTitle: 'Author Management',
    entityLabel: 'Author',
    baseFilters: [
      {type: 'documentType', value: 'data-record'},
      {type: 'sortBy', value: '-updated_at'}
    ],
    displayFilters: ['documentState', 'timeRange'],
    sort: '-updated_at',
    fields: ['metadata.*'],
    componentName: 'bluewinDashboardListItem'
  }],
  startPage: {
    path: '/articles'
  },
  mediaLibrary: {
    showUi: true, // default true
    altTextPrefilling: {
      metadataPropertyName: 'caption'
    }
    dashboard: {
      displayFilters: ['timeRange']
    },
    editorSelection: {
      displayFilters: ['timeRange']
    },
  }
}
```

## User Menu

Makes it possible to configure custom entries within the livingdocs user menu. If given a `userMenu.data.lastUpdate` property, it will visually indicate changes to the user. It is not possible to remove or alter the default livingdocs menu entries.

![Custom user menu](images/custom_user_menu.png)

## Main Navigation

Configures the main navigation see behind the burger menu in Livingdocs.
You can either configure a predefined `liItem`, link a custom dashboard or link an external page.

The possible values for `liItem` are: 'articles', 'pages', 'dataRecords', 'mediaLibrary', 'proofreading', 'lists', 'menus', 'tags' (imatrics), 'contentSetup', 'projectSettings', 'serverAdmin' (enterprise only)

For custom dashboards you configure the handle of your custom dashboard (see belwo) in the `dashboard` key. For external links you set the `href` property. For both you can define: `icon` (visual icon in the main nav), `group` (any of 'preferences', 'dashboards', 'custom', 'top') and `label` (visual title).

## Dashboards

The `dashboards` entry allows you to configure custom dashboards, e.g. for authors (data-records) or proofreading (tasks).

There are 3 `types` of custom dashboards (`type` property):
- `dashboard`
- `kanbanBoard`
- `taskBoard` (predefined `kanbanBoard` for a task)

Kanban Boards are very similar to dashboards, except they do have multiple result columns. Each result column will show a list of documents the same as a single dashboard does. The documents cannot be manually sorted or moved between columns, instead each column typically has its own filter settings.

For example a task board will show all tasks in the `requested` state in one column and tasks with the state `inProgress` and `done` in the other columns. In order to move a card into another column you simply have to open the document and move the task into another state.

### Common Dashboard Properties

Custom dashboards have some basic properties in common which are described in more detail below.

#### handle

Identifier for a custom dashboard. Is also used as a reference for the [main navigation](#main-navigation)

#### type

Type of the dashboard, one of these: `dashboard`, `kanbanBoard`, `taskBoard`

#### displayFilters

[Display Filters](./search/display_filter.md) are shown to the user below the search input

#### baseFilters

[Base Filters](./search/base_filter.md) are invisible filters and applied to every search (including the default result list)

#### sort

Sort the result, possible values are:
- `relevance` (default),
- `-created_at` / `created_at`,
- `-updated_at` / `updated_at`
- a metadata property e.g. `metadata.proofreading.priority`


### Example: Dashboard
```javascript
dashboards: [{
  handle: 'gallery-dashboard',
  type: 'dashboard',
  pageTitle: 'Gallery Board',
  // Label used to describe the documents in this Dashboard
  entityLabel: 'Article',
  // Invisible base filters applied to every search (including the default result list)
  baseFilters: [{type: 'documentType', value: 'article'}],
  // Filters shown to the user below the search input
  displayFilters: ['documentState', 'timeRange'],
  sort: '-updated_at',
  // fields to be returned from the server (not all metadata fields are returned by default)
  fields: ['metadata.*'],
  // Enterprise only: This is the name of the vue component used in the result list
  componentName: 'liHeroCard',
  // Enterprise only: The componentOptions are injected into the component
  // `liHeroCard` (in this example)
  componentOptions: {teaserImage: 'teaserImage'},
  // Enterprise only: CSS class set as a wrapper around the result list
  cssWrapper: 'li-result-columns'
}]
```

### Example: Taskboard (simple config)

```js
dashboards: [{
  handle: 'kanban-proofreading',
  type: 'taskBoard',
  pageTitle: 'Proofreading',
  // This is the name of a metadataProperty of `type: 'li-task-v2'`
  taskName: 'proofreading',
  displayFilters: ['documentState', 'timeRange']
}]
```

### Example: Kanbanboard (full config)

```js
dashboards: [{
  handle: 'kanban-proofreading',
  type: 'kanbanBoard',
  pageTitle: 'Proofreading',
  // Label used to describe the documents in this KanbanBoard
  entityLabel: 'Article',
  displayFilters: [],
  // Base filters are applied to all columns
  baseFilters: [{type: 'documentType', value: 'article'}],
  // This is the name of the angular component to use in all columns
  // (can also be defined for each columns separately)
  componentName: 'liTaskCard',
  // include all metadata properties in the search response data
  fields: ['metadata.*'],
  // Set the target when clicking on a card. Currently supported:
  // - 'article' (default setting)
  // - 'tasks'
  openState: 'tasks',
  showFooter: true,
  columns: [{
    handle: 'requested',
    label: 'Needs Proofreading',
    // Filter applied for this column on top of the `baseFilter`
    columnFilter: [{type: 'metadata', key: 'proofreading.state', value: 'requested'}],
    sort: [`metadata.proofreading.priority`, `metadata.proofreading.deadline`]
    // The componentOptions are injected into the component `liTaskCard` (in this example)
    componentOptions: {column: 'todo', taskName: 'proofreading'}
  }, {
    handle: 'in-progress',
    label: 'In Progress',
    columnFilter: [{type: 'metadata', key: 'proofreading.state', value: 'accepted'}],
    sort: [`metadata.proofreading.priority`, `-metadata.proofreading.accepted.date`],
    componentOptions: {column: 'doing', taskName: 'proofreading'}
  }, {
    handle: 'done',
    label: 'Finished Proofreading',
    columnFilter: [{type: 'metadata', key: 'proofreading.state', value: 'completed'}],
    sort: [`-metadata.proofreading.completed.date`],
    componentOptions: {column: 'done', taskName: 'proofreading'}
  }]
}]
```

## startPage

Set custom `startPage: {path: '/my-custom-path'}} to set the path used to render on login or when switching projects.

## Media Library

After linking the media library on the `mainNavigation` (see above), one can also define `displayFilters` to customise the media library dashboard (see example on top of the page).