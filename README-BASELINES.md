## Training Baselines

To run the LEO baseline using a given dataset configuration file:

```shell title="shell"
python src/leo.py --config configs/iscxvpn-2016-c13-bo.yml
```

Based on the LEO output printed to the console, update the LEO section in the configuration file for this dataset.

```yaml
leo:
  depths: []
  features: []

  pareto_front:
    - max_depth: 
      f1_score: 
      num_flows: 
      num_features:
      num_tcam_rules:
...
```

Similarly, to run the NetBeacon baseline:

```shell title="shell"
python src/netbeacon.py --config configs/iscxvpn-2016-c13-bo.yml
```

Based on the NetBeacon output printed to the console, update the NetBeacon section in the configuration file for this dataset.

```yaml
netbeacon:
  depths: []
  features: []

  pareto_front:
    - max_depth: 
      f1_score: 
      num_flows: 
      num_features:
      num_tcam_rules:
...
```

For the single partition (unpartitioned) design, a SpliDT-based baseline can be evaluated as following:

```shell title="shell"
python src/one_partition.py --config configs/iscxvpn-2016-c13-bo.yml
```

Based on the console output, update the one_partition section in the configuration file for this dataset.

```yaml
one_partition:
  depths: []
  features: []

  pareto_front:
    - max_depth: 
      f1_score: 
      num_flows: 
     num_features:
      num_tcam_rules:
...
```
