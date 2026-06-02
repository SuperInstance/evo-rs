# evo-rs

Evolutionary optimization in Rust. GA, DE, NSGA-II, GP. When gradient descent can't help you.

[![crates.io](https://img.shields.io/badge/crates.io-0.1.0-orange)](https://crates.io/crates/evo-rs)

[![docs](https://docs.rs/evo-rs/badge.svg)](https://docs.rs/evo-rs)

## What's inside

- **Genetic algorithms** — tournament/roulette selection, single-point/two-point/uniform crossover, gaussian/bitflip mutation
- **Differential evolution** — DE/rand/1, DE/best/1, DE/current-to-best/1 with adaptive F/CR
- **NSGA-II** — multi-objective optimization with crowding distance and non-dominated sorting
- **Genetic programming** — tree-based GP with subtree crossover, tournament selection

All components are modular — mix and match operators to build the optimizer your problem needs.

## Quick start

```toml
[dependencies]
evo-rs = "0.1"
```

### Genetic Algorithm

```rust
use evo_rs::{
    ga::{GAConfig, run_ga},
    individual::RealCodedIndividual,
    fitness::FitnessFn,
};

fn rastrigin(x: &[f64]) -> f64 {
    10.0 * x.len() as f64 + x.iter().map(|xi| xi * xi - 10.0 * (2.0 * std::f64::consts::PI * xi).cos()).sum::<f64>()
}

let config = GAConfig {
    pop_size: 200,
    generations: 500,
    mutation_rate: 0.1,
    crossover_rate: 0.8,
    ..Default::default()
};

let best = run_ga(config, 10, -5.12..5.12, |ind| -rastrigin(ind.genes()));
println!("Best fitness: {}", best.fitness());
```

### NSGA-II (multi-objective)

```rust
use evo_rs::nsga2::{NSGA2Config, run_nsga2};

let front = run_nsga2(NSGA2Config::default(), 30, 0.0..1.0, |ind| {
    let x = ind.genes()[0];
    vec![x.powi(2), (x - 1.0).powi(2)]
});
```

### Genetic Programming

```rust
use evo_rs::gp::{GPTree, GPTerminal};
```

## License

MIT OR Apache-2.0
