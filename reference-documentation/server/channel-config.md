# Channel Config

## Summary

The channel configuration allows you to:

* describe general information
* define contentTypes
* define copy options
* configure the push notifications feature
* configure the multi-language feature
* configure integrations
* configure includeServices. See [doc-includes](../../evaluation-guide/intro.md) for more information.

The contentType configuration is described in detail here: [contentType config](content-type-config.md)

## Example channel configuration

The following sample configuration file illustrates all of the above.

```javascript
{
  v: 2,

  settings: {
    // If a channel has the channel `name` 'web' the channel configuration with
    // the `handle` 'web' will be used.
    handle: 'web',
    editMode: 'default',

    // Multi-language
    // Important: Make sure to use ISO-639-1 compliant locale codes

    // defines the languages that a user can select for a document
    availableLanguages: [
      {
        'name': 'English',
        'value': 'en-US'
      }, {
        'name': 'German',
        'value': 'de-DE'
      }
    ],
    // used to create new documents
    defaultLanguage: {
      name: 'English',
      value: 'en-US'
    },

    // setup the push notification feature if applicable
    // for this to work you also must:
    // 1. configure firebase in the server-wide `pushNotifications` configuration
    // 2. have a metadata field with the (exact) name `pushNotifications`
    // IMPORTANT: value can not contain more than three comma separated entries
    // see firebase conditions for more details.
    pushNotifications: {
      topics: [{
        handle: 'breaking-news',
        label: 'Breaking News',
        value: 'ios_de_breakingnews, android_de_breakingnews'
      }, {
        handle: 'sport',
        label: 'Sport',
        value: 'ios_de_sports, android_de_sports'
      }]
    }

    // Copy Configuration
    copy: [{
      source: {
        channelHandle: 'web',
        contentType: 'gallery'
      },
      target: [{
        channelHandle: 'web',
        contentType: 'gallery',
        options: {
          copyUnknownComponents: false
        },
        metadata: {
          map: [
            {from: 'title', to: 'title'},
            'tasks',
            {from: 'author', to: 'description'}
          ]
        },
        allowTransform: true
      }]
    }],

    // This setting can be overwritten in the `contentType` config
    imageSourcePolicy: [{
      provider: 'upload',
      enabled: true
    }, {
      provider: 'hugo',
      enabled: false
    }, {
      provider: 'url',
      enabled: true,
      hosts: ['//pixabay.com']
    }],

    // includeServices (added in release-2020-05)
    // using this config, you can declaratively define include services
    includeServices: [
      {
        handle: 'my-service', 
        rendering: {
          type: 'remote', 
          url: 'http://example.com', 
          timeout: 3000
        }, 
        config: {
          foo: 'this config is sent to the service'
        },
        // paramsSchema is used to render a form for datainput when a component
        // using this service on an include directive
        paramsSchema: [
          {
            type: 'li-text', // li-text is the only supported type in release-2020-05
            handle: 'myText'
          }
        ], 
        defaultParams: {
          myText: 'A default value for the myText property'
        }
      }
    ],

    // Integrations (added in release-2020-04)
    integrations: {
      // Comyan (added in release-2020-04)
      comyan: {
        enabled: true,
        buttonLabel: 'open comyan'
        mediaSystem: {
          credentials: {
            username: 'user',
            password: '****'
          },
          baseUrl: 'https://example.com/MediaSystem',
        }
      }
    }

    // Webhooks (added in release-2020-05)
    webhooks: {
      enabled: true
      configurations: [{
        handle: 'my-webhook',
        label: 'My Webhook',
        description: 'A description for future self and coworkers',
        url: 'https://example.com/my-webhook-endpoint',
        token: 'a-secret-token-to-sign-the-request'
        active: true
        events: ['document.published', 'document.unpublished']
      }
    }]
  }

  },

  // Content Types
  contentTypes: [{
    require('./path/to/one/content_type_config'),
    require('./path/to/another/content_type_config')
  }]
}
```
