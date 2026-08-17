# Integrations

A machine-readable catalog of the other services an API already connects to — the
connector, app, or marketplace listing every SaaS platform publishes and nobody can query.

This is the schema behind the [Integrations property](https://apicommons.org/common/integrations/)
in [API Commons](https://apicommons.org).

## Why this one is worth doing

An integrations page is a wall of several hundred logos with a link under each. It is the
single best public statement a company makes about who it interoperates with, and it is
locked in HTML.

Made machine-readable, it becomes something you can diff against a competitor's, resolve
to the providers on the other end, and check for rot. Three fields do most of that work:

- **`partner_domain`** — the apex domain of the service on the other end. The
  highest-value field here. A name is ambiguous across a catalog of hundreds; a domain
  resolves to a company. It is what makes a catalog **joinable** to a provider index
  rather than merely readable. Catalog display names and catalog URLs drift apart
  constantly — "Active Directory" living at `/connectors/ldap/` — and the domain is the
  stable join key.
- **`built_by`** — first-party, partner, community, or third-party. On a marketplace page
  all four look identical. They carry completely different support expectations, and this
  is the difference between an integration that is maintained and one that will quietly
  stop working.
- **`count`** on the property — what the provider *claims* the catalog holds, recorded
  separately from how many entries were actually captured. When the two disagree the
  catalog was sampled, not harvested, and a reader deserves to know that rather than
  reading a partial capture as complete.

## Artifacts

- **[integrations-json-schema.yml](integrations-json-schema.yml)** — the JSON Schema (2020-12).
- **[integrations-example-1.yml](integrations-example-1.yml)** — a standalone catalog slice
  for a made-up integration platform.
- **[integrations-example-2.yml](integrations-example-2.yml)** — the `Integrations` property
  envelope, showing `count` against a deliberately partial capture.
- **[validate.py](validate.py)** — validates any document against the schema.

## Using it

```yaml
- name: Salesforce
  url: https://example.com/connectors/salesforce/
  partner_domain: salesforce.com
  category: CRM
  kind: connector          # connector | app | plugin | webhook | sdk | template
                           # | workflow | embed
  direction: bidirectional # inbound | outbound | bidirectional
  built_by: first-party    # first-party | partner | community | third-party
  status: ga
  auth: oauth2
```

Only `name` is required — some catalog entries are not companies at all (a generic
webhook, a CSV import), and the schema should not force a partner onto them.

Keep the provider's own `category` vocabulary rather than normalising it away. The
categories a company chooses are themselves evidence of how it sees its market.

## Validating

```
pip install jsonschema pyyaml
python3 validate.py integrations-example-1.yml
```

## Support

Questions, corrections, and requests go in
[the issues](https://github.com/api-commons/integrations/issues).

## License

Two licenses, by kind of thing:

- **Artifacts** — the schemas, rulesets, fixtures, examples and API descriptions — are
  **[CC BY-NC-SA 4.0](LICENSE)** (Attribution–NonCommercial–ShareAlike).
- **Code** — the validator, test harness and packaging — is **[Apache-2.0](LICENSE-CODE)**.

API Commons licenses **artifacts** under CC BY-NC-SA 4.0 and **code** under Apache-2.0.
