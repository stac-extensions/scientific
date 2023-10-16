# Scientific Citation Extension Specification

- **Title:** Scientific Citation
- **Identifier:** <https://stac-extensions.github.io/scientific/v1.0.0/schema.json>
- **Field Name Prefix:** sci
- **Scope:** Item, Collection
- **Extension [Maturity Classification](https://github.com/radiantearth/stac-spec/tree/master/README.md#extension-maturity):** Stable
- **Owner**: @m-mohr
- **History:** [Prior to March 30, 2021](https://github.com/radiantearth/stac-spec/commits/v1.0.0-rc.2/extensions/scientific)

This document explains the Scientific Extension to the
[SpatioTemporal Asset Catalog](https://github.com/radiantearth/stac-spec) (STAC) specification.

This extension adds the ability to indicate from which publication data originates and how
the data itself should be cited or referenced. Overall, it helps to increase reproducibility and citability.

Human-readable references and [DOIs](https://www.doi.org/) can be used in this extension. DOIs are
persistent digital interoperable identifier that uniquely identify for digital publications. They
can be registered at registration agencies affiliated with the
[International DOI Foundation](https://www.doi.org/).

This extension applies to STAC [Item](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md)
and STAC [Collections](https://github.com/radiantearth/stac-spec/tree/master/collection-spec/collection-spec.md).
As these citation information are often closely bound to the Collection level and therefore are shared across all items,
it is recommended adding the fields to the corresponding Collection.

- Examples:
  - [Item](examples/item.json): The extension in a STAC Item
  - [Collection](examples/collection.json): The extension in a STAC Collection
  - [Collection (Assets)](examples/collection-assets.json): The extension in a STAC Collection with assets
  - [Collection (Item Asset Definition)](examples/collection-item-assets.json): The extension in a STAC Collection in Item Asset Defintions
  - [Collection (Summaries)](examples/collection-summaries.json): The extension in a STAC Collection in Summaries
- [JSON Schema](json-schema/schema.json)
- [Changelog](./CHANGELOG.md)

## Item Properties and Collection Fields

For Items, the fields are placed in the `properties`. For Collections, the fields are placed on the top level of the Collection.

| Field Name       | Type                 | Description |
| ---------------- | -------------------- | ----------- |
| sci:doi          | string               | The DOI of the data, e.g. `10.1000/xyz123`. This MUST NOT be a DOIs link. For all DOI names respective DOI links SHOULD be added to the links section (see chapter "Relation types"). |
| sci:citation     | string               | The recommended human-readable reference (citation) to be used by publications citing the data. No specific citation style is suggested, but the citation should contain all information required to find the publication distinctively. |
| sci:publications | [[Publication Object](#publication-object)] | List of relevant publications referencing and describing the data. |
| sci:orcids        | \[string]            | An array of Open Researcher Contribution IDs ([ORCID](https://orcid.org)) associated with this product. For all ORCIDs a link SHOULD be added to the links section.     |
| sci:ror s         | \[string]            | An array of Research Organization Record ([ROR](https://ror.org)) entity name or unique identifier. For all ROR(s) names and identifiers a link SHOULD be added to the links section. |

*At least one of the fields must be specified.*

### Publication Object

| Field Name | Type   | Description |
| ---------- | ------ | ----------- |
| doi        | string | The DOI of a publication referencing the data. This MUST NOT be a DOIs link. |
| citation   | string | Citation of a publication referencing the data. |

**doi** - The DOI name of a publication which describes and references the data. The publications
should include more information about the data and how it was processed. This MUST NOT be a DOI
link. For all DOI names respective DOI links SHOULD be added to the links section
(see chapter "Relation types").

**citation** - Human-readable reference (citation) of a publication which describes and references
the data. The publications should include more information about the data and how it was
processed. No specific citation style is suggested, but a citation should contain all information
required to find the publication distinctively.

## Relation types

The following types should be used as applicable `rel` types in the
[Link Object](https://github.com/radiantearth/stac-spec/tree/master/item-spec/item-spec.md#link-object).

| Type    | Description |
| ------- | ----------- |
| author | An ORCID link SHOULD be added to the links section for the author(s) referenced by the `sci:orcids` property with the `rel` type `author`. (see [rel type author](https://html.spec.whatwg.org/multipage/links.html#link-type-author)) |
| cite-as | A DOI link SHOULD be added to the links section for the publication referenced by the `sci:doi` property with the `rel` type `cite-as` (see the [RFC 8574](https://tools.ietf.org/html/rfc8574)). |

## Contributing

All contributions are subject to the
[STAC Specification Code of Conduct](https://github.com/radiantearth/stac-spec/blob/master/CODE_OF_CONDUCT.md).
For contributions, please follow the
[STAC specification contributing guide](https://github.com/radiantearth/stac-spec/blob/master/CONTRIBUTING.md) Instructions
for running tests are copied here for convenience.

### Running tests

The same checks that run as checks on PR's are part of the repository and can be run locally to verify that changes are valid. 
To run tests locally, you'll need `npm`, which is a standard part of any [node.js installation](https://nodejs.org/en/download/).

First you'll need to install everything with npm once. Just navigate to the root of this repository and on 
your command line run:
```bash
npm install
```

Then to check markdown formatting and test the examples against the JSON schema, you can run:
```bash
npm test
```

This will spit out the same texts that you see online, and you can then go and fix your markdown or examples.

If the tests reveal formatting problems with the examples, you can fix them with:
```bash
npm run format-examples
```
