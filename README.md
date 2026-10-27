---
# 🛡️CPQ-Vehicular-FRL: Communication-Efficient Private Federated Reinforcement Learning with Post-Quantum Authentication for Vehicular Networks

CPQ-Vehicular-FRL is a framework for private and communication-efficient federated reinforcement learning (FRL)
in vehicular networks. Vehicles learn a cooperative routing policy with federated deep Q-learning in SUMO and
exchange their models with a road-side unit (RSU) over a packet-level IEEE 802.11p channel simulated in ns-3.
Model updates are protected with correlated private quantization (CPQ), which uses complementary shared
randomness between paired vehicles to reduce the aggregation error at the same local differential privacy
guarantee and one-bit communication cost as independent quantization, and are signed with post-quantum
signatures.

![Architecture](docs/architecture.png)

📘 Paper: arXiv link to be added

📦 [Run outputs on Zenodo](https://doi.org/10.5281/zenodo.23693204)

**Key Features:**
- 🔐 **Correlated private quantization (CPQ, CPQ-λ)**: one-bit model updates with local differential privacy and lower aggregation error than independent quantization (LDP-FL).
- ⚡ **Communication-efficient uploads**: one-bit updates reduce the authenticated uplink by 92.4% compared with full-precision updates.
- 🛡️ **Post-quantum authentication**: model updates signed with ML-DSA-44, Falcon-512 or SLH-DSA-SHA2-128f.
- 🚗 **Mobility-coupled simulation**: SUMO traffic coupled with packet-level 802.11p vehicle-to-infrastructure communication in ns-3.
- 🕵️ **Privacy evaluation**: a vehicle-origin inference attack on the updates received by the server.

---

## 🛠️ Requirements
- Python 3.10, TensorFlow 2.21, Flower 1.29
- SUMO 1.26.0
- liboqs 0.15.0 with liboqs-python (post-quantum signatures)
- ns-3.46 (only for the experiments over IEEE 802.11p, see [ns3/README.md](ns3/README.md))

Tested on Ubuntu 20.04.

## 📦 Environment Setup
```bash
git clone https://github.com/solidlabnetwork/cpq-vehicular-frl.git
cd cpq-vehicular-frl
conda create -n cpq-frl python=3.10
conda activate cpq-frl
pip install -r requirements.txt
export SUMO_HOME="$(python -c 'import sumo, os; print(os.path.dirname(sumo.__file__))')"
```

Build the liboqs C library:
```bash
git clone --branch 0.15.0 --depth 1 https://github.com/open-quantum-safe/liboqs
cmake -S liboqs -B liboqs/build -DBUILD_SHARED_LIBS=ON -DCMAKE_INSTALL_PREFIX=$HOME/oqs_install
cmake --build liboqs/build --parallel 8 && cmake --install liboqs/build
export OQS_INSTALL_PATH=$HOME/oqs_install
export LD_LIBRARY_PATH=$HOME/oqs_install/lib:$LD_LIBRARY_PATH
```

## 🔧 Single Run
```bash
FL_SEED=1 FL_OUT_DIR=results/test python run_shared.py --config configs/base.yaml
```

## 🔧 Experiments of the Paper
```bash
python tools/run_experiment.py E1        # also LAMBDA, POPULATION, E2, E3, ATTACK
python tools/analyze_experiment.py experiments/final/E1_privacy_utility
```

## 📊 Tables and Figures
From your own runs or from the run outputs on [Zenodo](https://doi.org/10.5281/zenodo.23693204) (unzipped in the
repository root):
```bash
pip install -r requirements-analysis.txt
python tools/paper_results.py
bash figures/make_all.sh --data results/paper/figure_data.json
```

## 📚 Citation

```bibtex
@article{salehi2026cpq,
  title={Communication-Efficient Private Federated Reinforcement Learning with Post-Quantum Authentication for Vehicular Networks},
  author={Salehi, Hojat Allah and Aras, Devrim and Mia, Md Jueal and Amini, M. Hadi and Karim, Imtiaz and Chowdhury, Mashrur and Thuraisingham, Bhavani},
  year={2026}
}
```

## 📄 License

MIT, see [LICENSE](LICENSE).
