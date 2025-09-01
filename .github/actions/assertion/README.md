# ns-assertion-action

`ns-assertion-action` is a GitHub Action for generating assertion documents.

The `ns-assertion-action` generates the assertion document for the Go binary built in the pipeline.

## Data flow

![assertion](img/assertion3.png)

## Prerequisites

The action takes the input string representing output from the `go version -m <binary>` command.
The command retrospects the Go binary and lists included Go packages.

## Configuration

### Required input data

The action require text input data representing raw output from the Go command `go version -m <binary>`.

Example:

```shell

```

### Required coniguration parameters

## Troubleshooting
