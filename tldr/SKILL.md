---
name: tldr
description: "Bikin ringkasan singkat dari session yang lagi jalan. Dua bagian: kerjaan yang udah selesai dan status task terakhir. Trigger: user bilang 'tldr', 'recap', 'summarize', 'where are we', 'udah ngapain aja', atau /tldr."
---

# TLDR — Session Recap

Bikin ringkasan cepat dari session. Formatnya ringkas, scannable, dan langsung ke inti.

## Gaya Bahasa

- **Indo/English campur** — sesuai gaya obrolan kita, bukan formal kaku
- **Santai tapi informatif** — bukan essay, bukan juga robot
- **Singkat, langsung ke poin** — orang baca recap buat cepet tau, bukan buat baca novel
- Tetap profesional di bagian teknis (file path, commit, command — tulis apa adanya)

## Format Output

```
## Done
- <action> <object>. <result>.
- ...

## Current task
- Task: <nama>
- Status: <complete | in progress | blocked>
- Last step: <terakhir ngapain>
- Next step: <selanjutnya ngapain>
- Blocker: <kenapa blocked> (kalau statusnya blocked)
```

## Aturan

### Done (kerjaan selesai)
- **Maksimal 5 bullets.** Satu baris per bullet.
- **Maksimal 15 kata per bullet.**
- **Group yang related** — jangan satu bullet per commit. Kalau 3 commit buat fix yang sama, jadiin satu bullet.
- **Tulis hasilnya aja**, bukan prosesnya. "Fixed prayer times bug" bukan "Saya cek log, ternyata ada error di line 42, terus saya ubah..."
- **Sertakan reference** kalau ada:
  - Commit: [`0d43184`](https://github.com/org/repo/commit/0d43184)
  - PR: [#446](https://github.com/org/repo/pull/446)
  - File: [`services/prayerTimes.ts`](services/prayerTimes.ts)
  - File + line: [`services/prayerTimes.ts:42`](services/prayerTimes.ts#L42)
  - Kalau ga tau URL-nya, tulis aja plain identifier. Jangan ngarang URL.

### Current task (task terakhir)
- **Maksimal 5 field** (Task, Status, Last step, Next step, Blocker)
- Satu baris per field
- Blocker cuma ditulis kalau statusnya `blocked`

### Hard limits
- **Total maksimal 20 baris** (Done + Current task gabung)
- Kalau kebanyakan, keep yang **highest impact** aja
- Ga ada table, sub-bullets, atau code block di recap
- Hapus narasi tool call — cuma tulis hasilnya

### Konteks khusus
- Kalau ga ada kerjaan selesai: tulis **"Belum ada yang selesai."** — jangan ngarang
- Kalau session baru mulai: tulis **"Session baru, belum ada kerjaan."**
- Recap ini buat **session yang lagi jalan aja**, bukan gabungan semua session

## Contoh

```
## Done
- Fixed duplicate adzan notifications. Debounce guard 4 detik. [`a30199c`](https://github.com/bipisurel-bps/Jadda-android-apps/commit/a30199c)
- Updated doa100.json translations. 100 doa, 0 duplikat.
- Built APK v1.3.0-vc15. [Download](https://gofile.io/d/CEvFA4)

## Current task
- Task: Deploy ke Play Store
- Status: blocked
- Last step: APK udah di-build, upload ke gofile
- Next step: Upload AAB ke Google Play Console
- Blocker: EAS free tier limit habis, nunggu reset
```
