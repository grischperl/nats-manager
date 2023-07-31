# Governance

Some quality aspects are covered by automated verification, so you must locally execute tooling before a commitment.
This document aims to guide through the development flow.

## Modifying the API definitions

This project makes use of the [controller-gen](https://book.kubebuilder.io/reference/controller-gen.html) tool provided by [Kubebuilder](https://book.kubebuilder.io/).
In order to modify the API definitions it is necessary to adapt the ["marker comments"](https://book.kubebuilder.io/reference/markers.html) in the
Go code.

Additionally, this project uses [validation markers](https://book.kubebuilder.io/reference/markers/crd-validation.html) to provide CRD validation and defaulting via [OpenAPI v3 schemas](https://github.com/OAI/OpenAPI-Specification/blob/main/versions/3.0.0.md#schemaObject).
The rules are written using the [Common Expression Language](https://github.com/google/cel-spec).
For further information and examples look to the [Kubernetes documentation](https://kubernetes.io/docs/tasks/extend-kubernetes/custom-resources/custom-resource-definitions/#validation) of validation rules
and the CEL [language definition](https://github.com/google/cel-spec/blob/v0.10.0/doc/langdef.md).

After the modifications run the following command to generate the new manifests such as CRs and CRDs:
   ```sh
   make manifests
   ```

If changes to the `runtime.Object` interface were made, the `DeepCopy` functions need to be updated as well.
The generation is controlled by [Kubebuilder markers](https://book.kubebuilder.io/reference/markers/object.html?highlight=deep#objectdeepcopy).
   ```sh
   make generate
   ```

## Sourcecode Linting

The quality of this project is ensured by source code linting using [golangci-lint](https://golangci-lint.run/).

To fix common lint issues, run the following command:

   ```sh
   make imports-local
   make fmt-local
   ```

To run thorough lint checking, execute the followng command:

   ```sh
   make lint-thoroughly
   ```

If necessary, lint warnings can be ignored. However, if this is desired a comment explaining the reason must be provided,
according to the following format:

`//no-lint:<LINTER> // <REASON>`