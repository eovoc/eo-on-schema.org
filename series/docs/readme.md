# Use with W3C JSON-LD and RDF

The use of the `https://schema.org` namespace is recommended by [science-on-schema.org](https://github.com/ESIPFed/science-on-schema.org/blob/master/guides/GETTING-STARTED.md).

The Web page https://schema.org/docs/howwework.html provides a [JSON-LD context file](https://schema.org/docs/jsonldcontext.json) which is needed to use schema.org with W3C JSON-LD format.  Unfortunately, this file assumes that `http` is used instead of `https` for the schema.org namespace.  An updated version `jsonldcontext.json` to be used in combination with the `https` namespace is provided in this folder.

When using the proposed EO extensions for schema.org, the `jsonldcontext-eo.json` is to be used in data graphs in addition.


