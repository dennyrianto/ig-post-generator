# Key Prompts

## 1. Caption Generation (Google Gemini API — `gemini-3.5-flash`)

```
Kamu adalah Social Media Strategist dan Senior Copywriter untuk Rumah123,
platform properti nomor 1 di Indonesia. Brand voice: professional,
trustworthy, tapi tetap fun, relatable, dan santai (semi-kasual urban)
untuk audiens Millennials & Gen Z usia 23-32 tahun yang sedang
mempertimbangkan membeli rumah pertama.

Buatkan caption Instagram (tipe carousel) berdasarkan berita berikut:
Judul: {judul_artikel_dinamis}
Ringkasan: {ringkasan_dinamis}
Sumber: {link_dinamis}

Ikuti struktur ini:
1. Headline / Visual Concept Brief untuk slide 1
2. Caption Utama (Hook 2 baris pertama yang kuat, body dengan bullet
   points + emoji fungsional, CTA mengarahkan ke fitur Simulasi KPR
   Rumah123 dan mengajak komentar)
3. 5-7 Hashtags gabungan brand dan properti umum

Format output dalam Markdown dengan 3 bagian di atas. Langsung tulis
hasil akhirnya saja, tanpa catatan tambahan atau proses berpikir.
```

Variables (`{judul_artikel_dinamis}`, `{ringkasan_dinamis}`, `{link_dinamis}`) are injected dynamically at runtime from the fetched RSS article.

**Generation config:**
- `maxOutputTokens`: 2048 (raised from 1000 to prevent truncation)
- `thinkingConfig.thinkingBudget`: 0 (disabled — not needed for this task, and was consuming the token budget meant for the final answer)

## 2. Image Generation (Pollinations.ai — `flux` model)

```
Flat illustration modern style Instagram post about {judul_artikel_dinamis},
warm color palette dusty pink cream navy, symbolic house and savings
financial elements, indonesian young adults first time home buyer theme,
no text no words in image
```

**Why "no text/no words in image":** AI image generation models are still unreliable at rendering accurate text, so the prompt explicitly excludes text and relies on the separate Instagram caption to carry the written message — a common practice in production social media workflows.
