## Labels

<b>Labels</b> are key-value pairs attached to objects such as Pods, ReplicaSets, etc.

They are used to organize and select a subset of objects, based on requirements in place.

Labels are used for grouping, not uniqueness.

![Pod Labels](./imgs/kb_labels.png)

## Label Selectors

Controllers use <b>Label Selectors</b> to select a subset of objects.

- Equality-based selectors
    - Matching achieved with `==` or `!=`
    - `env==dev` or `env!=dev`
- Set-based selectors
    - Matching achieved with `in`, `notin` for values
    - `exist` and `does not exist` for keys
    - `env in (dev,qa)`
