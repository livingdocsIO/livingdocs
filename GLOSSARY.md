## Livingdoc

A representation of content. It separates content from structure by using
`Components`. A `Livingdoc` references a specific `Design` wich defines the
available `Components` that can be used in the document.

## Design

A list of `Components` and configurations how they can be used
in a `Livingdoc` and how users can interact with these `Components` in the
`Livingdocs Editor`.

## Project

Represents an organisation and consists of users and channels.

## Channel

Represents a collection of documents. All documents in a `channel` must use
the same `design`. A `channel` also defines the `Renditions`.

## HuGO

A stand-alone product to manage images and agency reports in large numbers.
Also acts as a proxy for our Print Api to print publishing systems like `NewsNT`.

## NewsNT

A print publishing system.