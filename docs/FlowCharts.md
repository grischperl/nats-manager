# NATS Manager

## Concept
```mermaid
graph TD

  helm -->Controller
  Controller -->helm
  subgraph helm [NATS Helm Charts]
    Cluster[NATS Cluster]
    Service[NATS Service]
  end

  subgraph module [NATS Module]
    CR[NATS CR]-->Controller
    Controller -->CR
    subgraph manager [NATS Manager]
      Controller[NATS-Controller]
    end
  end
```

### NATS Manager is started
```mermaid
graph LR
  A(Start NATS manager) -->|Controller| D(Creates, watches & reconciles resources: CM, Sec, Sv, SfS, DR)
```

### NATS Manager reacts to NATS CR changes
```mermaid
graph LR
  E(NATS CR changes)-->F(Reconciliation triggered)-->|Controller|G(Resources are adapted to reflect the changes)
```

### NATS Manager reacts to resource changes
```mermaid
graph LR
  A(Resource changes/deleted)-->B(Reconciliation triggered)-->|Controller|C(Resources are restored according to their owner: NATS CR)
```

### Overview: NATS Manager watches resources
```mermaid
graph TD
  Con[NATS-Controller] -->|watches| cm[ConfigMap]
  Con -->|watches| sc[Secret]
  Con -->|watches| sv[Service]
  Con -->|watches| sfs[StatefulSet]
  Con -->|watches| dr[DestinationRule]
```