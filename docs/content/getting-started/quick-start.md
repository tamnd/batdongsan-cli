---
title: "Quick start"
description: "Fetch your first record with batdongsan."
weight: 30
---

Once `batdongsan` is on your `PATH`, fetch a page. The argument is the path
of the page on batdongsan.com (everything after the host), or a full URL:

```bash
batdongsan page <path>
```

By default you get an aligned table. Ask for JSON when you want to pipe it:

```bash
$ batdongsan page <path> -o json
[
  {
    "id": "<path>",
    "url": "https://batdongsan.com/<path>",
    "title": "<path>",
    "body": "..."
  }
]
```

## Shape the output

The same flags work on every command:

```bash
batdongsan page <path> --fields id,url        # keep only these columns
batdongsan page <path> --template '{{.Body}}' # just the body text
batdongsan page <path> -o jsonl | jq .url     # one object per line, into jq
```

`-o` takes `table`, `json`, `jsonl`, `csv`, `tsv`, `url`, or `raw`. Left to
`auto`, it prints a table to a terminal and JSONL into a pipe, so the same
command reads well by hand and parses cleanly downstream. See
[output formats](/reference/output/) for the full contract.

## Follow the links

`links` lists the pages a page links to, and each one is a path you can fetch in
turn:

```bash
batdongsan links <path> -n 10                 # the first ten links
batdongsan links <path> -o url                # just the URLs
batdongsan links <path> -o url | head -3 | xargs -n1 batdongsan page
```

## Serve it instead

The same operations are available over HTTP and to agents over MCP:

```bash
batdongsan serve --addr :7777 &
curl -s 'localhost:7777/v1/page/<path>'          # NDJSON, one record per line
batdongsan mcp                                # MCP over stdio: page, links
```

## What to build next

This scaffold ships one example type, `page`, wired end to end so the whole
chain works today. To make it really about batdongsan, model the records you
care about in `batdongsan/` and declare their operations in
`batdongsan/domain.go`. Each one you add shows up as a command here, a route
under `serve`, and a tool under `mcp`, with no extra wiring. The
[guides](/guides/) cover the common jobs.
