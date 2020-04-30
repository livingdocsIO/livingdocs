# Image Source Policy Config

## Source Policy

Different sources for images can be enabled and disabled. E.g. it is possible to disable image uploads by users and only allow e.g to use images from HuGo.

This config can be defined per project and overwritten per contentType. \(It is also possible to configure it generally in the editor\).

Example config with multiple sources:

```javascript
{
  sourcePolicy: [{
    provider: 'upload',
    enabled: true
  }, {
    provider: 'hugo',
    enabled: true
  }, {
    provider: 'url',
    enabled: true,
    hosts: ['https://cdn.pixabay.com']
  }]
}
```

## Server Configuration

This config can be defined as `imageSourcePolicy` in these places:

For a project: [Settings configuration](../server/channel-config.md) Overwrite per contentType: [ContentType configuration](../server/content-type-config.md)

## Editor Configuration

This config can be placed into the `editor` config If both are defined the project or `contentType` specific configs will take precedence.

Editor config:

```javascript
{
  images: {
    sourcePolicy: [
      // ...
    ]
  }
}
```

## Default Config

This whole config is optional. By default upload and hugo are enabled and url is disabled.

Current default policy:

```javascript
const defaultPolicies = {
  upload: {enabled: true},
  hugo: {enabled: true},
  url: {enabled: false}
}
```
