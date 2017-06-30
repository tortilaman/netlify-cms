# Configuring your site

## Widgets

Widgets define the data type and interface for entry fields. Netlify CMS comes with several built-in widgets, including:

Widget | UI | Data Type
--- | --- | ---
`string` | text input | string
`boolean` | select input | switch for true and false
`text` | text area input | plain text, multiline input
`number` | text input with `+` and `-` buttons | number
`markdown` | rich text editor with raw option | markdown-formatted string
`datetime` | date picker widget | ISO date string
`image` | file picker widget with drag-and-drop | file path saved as string, image uploaded to media folder
`hidden` | No UI | Hidden element, typically only useful with a `default` attribute
`list` | text input | strings separated by commas
`file` | file input | input file upload
`select` | select input | dropdown filled with `options` from the config
`object` | custom | `fields` as a list defined in the config
`relation` | autocomplete text input | string

We’re always adding new widgets, and you can also [create your own](/docs/extending).

### Relation Widget

The relation widget allows you to reference an existing entry from within the entry you're editing. It provides a search input with a list of entries from the collection you're referencing, and the list automatically updates with matched entries based on what you've typed.

The following field configuration properties are specific to fields using the relation widget:

Property | Accepted Values | Description
--- | --- | ---
`collection` | string | name of the collection being referenced
`searchFields` | list | one or more names of fields in the referenced colleciton to search for the typed value
`valueField` | string | name a field from the referenced collection whose value will be stored for the relation
`name` | text input | string

Let's say we have a "posts" collection and an "authors" collection, and we want to select an author for each post - our config might look something like this:

```yaml
collections:
  - name: authors
    label: Authors
    folder: "authors"
    create: true
    fields:
      - {name: name, label: Name}
      - {name: twitterHandle, label: "Twitter Handle"}
      - {name: bio, label: Bio, widget: text}
  - name: posts
    label: Posts
    folder: "posts"
    create: true
    fields:
      - {name: title, label: Title}
      - {name: body, label: Body, widget: markdown}
      - name: author
        label: Author
        widget: relation
        collection: authors
        searchFields: [name, twitterHandle]
        valueField: name
```

