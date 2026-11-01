# ClearMic App 0.2.0

**Kya hai ye:** tumhare mic ki awaz ko saaf karke game/app tak pohchane wala
local audio processor. Ye XMicInject ke documented TCP protocol par chalta
hai (127.0.0.1:38673, 16 kHz mono PCM) — v0.1.0 wala hi protocol, lekin ab
andar **asli AI noise suppression (RNNoise)** bhi hai.

## v0.2.0 me naya kya hai

- **RNNoise AI suppression** — Xiph ka RNN model, wahi family jo Discord/OBS
  ki noise suppression me istemal hoti hai. Pankha, AC, traffic jaisi noise
  ko awaz se alag karke dabata hai. (Measured locally: noise-only hisay me
  ~70 dB kami — neeche test report dekho.)
- RNNoise ka apna speech-probability score ek soft gate chalata hai — jab
  tum nahi bol rahe, bachi hui noise bhi neeche chali jati hai.
- **Behtar DSP fallback** — agar RNNoise library na mile to bhi naya DSP
  chain (hysteresis + hold-time wala gate, slow AGC) v0.1.0 se behtar hai.
  (Measured: ~6.8 dB noise kami, awaz mehfooz.)
- AGC: tumhari awaz ka level khud aik target par rakhta hai.

## Install (Termux, sirf pehli dafa)

Is poori `ClearMic-App` folder ko phone ke home folder me rakho, phir:

```sh
cd ~/ClearMic-App
bash install.sh
```

Installer Python + numpy lagata hai aur RNNoise ko phone ke liye build
karne ki koshish karta hai (kuch minutes lag sakte hain). Build fail bhi
ho jaye to app DSP mode me chalegi — install rukega nahi.

## Chalana

```sh
bash start.sh
```

Ya: Termux:Widget se **ClearMic** shortcut — aik tap me start.
Khelte waqt Termux khula rakho. Screen par `[audio] ... vad=...` lines
ayein to samjho processing live hai.

## PUBG se jodne ke liye (alag hissa)

Ye app sirf awaz process karti hai. PUBG ke mic tak pohchane ke liye
XMicInject LSPosed module alag se chahiye (root + Magisk/Zygisk +
LSPosed wala setup, module ka scope sirf PUBG par). Module connect ho to
ClearMic ki screen par `[+] XMicInject connected` nazar ayega.

## Tuning

```sh
python src/clearmic_provider.py --gain 1.4 --strength 0.8
python src/clearmic_provider.py --rnnoise off     # sirf DSP
python src/clearmic_provider.py --self-test       # offline test
```

- `--gain`: 0.5–3.0 (awaz kitni unchi)
- `--strength`: 0.0–1.0 (gate kitna sakht; zyada sakht = halke alfaaz katne ka risk)
- `--rnnoise`: auto (default) / on / off

## Sach ye hai (limits)

- RNNoise steady noise (pankha, AC, generator) par behtreen hai; aas paas
  logo ki batain aur TV poori tarah nahi rok sakta.
- Asli phone/PUBG test abhi hona baqi hai — pehle kisi test account par
  try karna, phir asal istemal.
- Ye source kisi bhi app ko hook nahi karta aur koi anti-cheat bypass
  nahi hai — ye sirf aik local audio processor hai.
