<script setup>
import { computed, onMounted, onUnmounted, ref } from "vue";

const pages = [
  {
    label: "Prolog",
    title: "Takdir, Keputusan, dan Perpisahan",
    quote: "Barangkali, setiap perpisahan hanya memperlihatkan hal-hal yang terlambat kita mengerti",
    left: [
      "Tidak semua cerita diciptakan untuk dimiliki selamanya. Kita selalu mengira perpisahan terjadi pada hari seseorang memilih pergi.",
      "Padahal, perpisahan sering kali dimulai jauh sebelum itu. Saat percakapan berubah menjadi tebakan. Saat ketakutan lebih sering dipendam daripada dibagikan. Saat dua orang yang saling mencintai mulai memikul beban masing-masing tanpa pernah benar-benar saling melihat seberapa berat beban itu."
    ],
    right: [
      "Cerita ini bukan tentang mencari siapa yang salah. Ini adalah catatan tentang dua manusia yang pernah saling mencintai, namun perlahan kehilangan cara untuk saling memahami.",
      "Aku tidak menulisnya untuk mengubah masa lalu. Aku menulisnya agar suatu hari nanti aku mampu melihat kisah ini dengan lebih jujur, dan semoga siapa pun yang membacanya dapat menemukan bagian dari dirinya sendiri."
    ]
  },
  {
    label: "Bab 1",
    title: "Rumah I",
    quote: "Rumah tidak selalu berupa tempat. Kadang, ia datang dalam bentuk seseorang yang membuat kita ingin pulang.",
    left: [
      "Aku tidak jatuh cinta ketika hidup sedang baik-baik saja. Aku jatuh cinta ketika dunia sedang membuatku kehilangan arah.",
      "Saat itu aku tidak benar-benar tahu sedang mencari apa. Aku hanya menjalani hari demi hari seperti kebanyakan orang. Kuliah, bekerja, bertemu teman, lalu mengulang semuanya lagi keesokan harinya.",
      "Hidup terus berjalan, tetapi entah mengapa rasanya tidak pernah terasa utuh.",
    ],
    right: [
      "Lalu aku bertemu denganmu, Aku tidak jatuh cinta pada pandangan pertama. Aku jatuh cinta karena melihat seseorang yang mampu bertahan dengan caranya sendiri.",
      "Seseorang yang tetap bisa tersenyum, bahkan ketika hidup tidak selalu memberinya alasan untuk melakukannya."
    ]
  },
  {
    label: "Bab 2",
    title: "Rumah II",
    quote: "Ada orang yang mengajarkan kita cara mencintai. Ada pula orang yang mengajarkan bahwa kita pantas dicintai.",
    left: [
      "Aku tidak pernah tahu bahwa seseorang bisa begitu tulus memperjuangkan, begitu sabar memahami, dan begitu sungguh-sungguh membuatku merasa berarti.",
      "Selama hidupku, cinta selalu terdengar seperti kata yang sering diucapkan banyak orang, Namun setelah mengenalmu, aku mengerti bahwa cinta ternyata bisa hadir dalam hal-hal yang nyaris tidak terdengar.",
      "Dalam perhatian yang tidak pernah kuminta, dalam usaha yang tidak pernah terhitung.",
      "Dan perlahan, aku mulai terbiasa dengan semua itu.",
    ],
    right: [
      "Saat itu aku hanya menikmati kehangatannya, tanpa benar-benar memahami perjuangan di baliknya.",
      "Dan mungkin, Karena itu adalah pertama kalinya aku dicintai sedalam itu, aku tidak pernah benar-benar tahu bagaimana cara merayakanmu kembali."
    ]
  },
];

const nextChapters = [
  {
    label: "Bab 3",
    title: "Rumah III ",
    note: ""
  },
  {
    label: "Bab 4",
    title: "Untitled",
    note: "."
  },
  {
    label: "Bab 5",
    title: "Untitled",
    note: "."
  }
];

const currentPage = ref(0);

const page = computed(() => pages[currentPage.value]);
const progress = computed(() => Math.round(((currentPage.value + 1) / pages.length) * 100));

function goToPage(index) {
  currentPage.value = Math.max(0, Math.min(index, pages.length - 1));
}

function handleKeydown(event) {
  if (event.key === "ArrowLeft") goToPage(currentPage.value - 1);
  if (event.key === "ArrowRight") goToPage(currentPage.value + 1);
}

onMounted(() => {
  window.addEventListener("keydown", handleKeydown);
});

onUnmounted(() => {
  window.removeEventListener("keydown", handleKeydown);
});
</script>

<template>
  <main class="reader" aria-label="Tentang hal-hal yang terlambat kami mengerti">
    <aside class="shelf" aria-label="Daftar bab">
      <div class="brand">
        <div>
          <p>Last Chapter</p>
          <span>Tentang hal-hal yang terlambat kami mengerti</span>
        </div>
      </div>

      <nav class="chapters" aria-label="Navigasi bab">
        <button
          v-for="(chapter, index) in pages"
          :key="chapter.title"
          class="chapter-button"
          :class="{ 'is-active': index === currentPage }"
          type="button"
          @click="goToPage(index)"
        >
          <span>{{ chapter.label }}</span>
          <small>{{ chapter.title }}</small>
        </button>
      </nav>

      <div class="reading-meta">
        <span>Progress</span>
        <strong>{{ progress }}%</strong>
        <div class="progress-track" aria-hidden="true">
          <div :style="{ width: `${progress}%` }"></div>
        </div>
      </div>
    </aside>

    <section class="book-area">
      <div class="rotate-hint" role="note">
        <strong>Lebih nyaman dibaca landscape.</strong>
        <span>Putar layar untuk rasa ebook dua halaman, atau lanjut portrait untuk mode baca satu kolom.</span>
      </div>

      <header class="toolbar">
        <button
          class="icon-button"
          type="button"
          aria-label="Halaman sebelumnya"
          title="Halaman sebelumnya"
          :disabled="currentPage === 0"
          @click="goToPage(currentPage - 1)"
        >
          <span aria-hidden="true">‹</span>
        </button>

        <div class="book-title">
          <span>{{ page.label }}</span>
          <strong>{{ page.title }}</strong>
        </div>

        <div class="tool-group">
          <button
            class="icon-button"
            type="button"
            aria-label="Halaman berikutnya"
            title="Halaman berikutnya"
            :disabled="currentPage === pages.length - 1"
            @click="goToPage(currentPage + 1)"
          >
            <span aria-hidden="true">›</span>
          </button>
        </div>
      </header>

      <article class="book" aria-live="polite">
        <section class="cover-panel">
          <img
            src="/assets/story-cover.bmp"
            alt="Sampul seorang wanita yang berjalan pergi dalam keheningan sambil memikul tanggung jawab besar"
          />
          <div class="cover-illustration" aria-hidden="true">
            <span class="moon"></span>
            <span class="path"></span>
            <span class="woman">
              <i class="head"></i>
              <i class="hair"></i>
              <i class="body"></i>
              <i class="burden"></i>
              <i class="leg leg-one"></i>
              <i class="leg leg-two"></i>
            </span>
          </div>
          <div class="cover-copy">
            <span>Vol. 01</span>
            <h1>Last Chapter</h1>
            <p>Seorang wanita pergi dalam diam, membawa hal-hal yang tidak semua orang sanggup melihat beratnya.</p>
          </div>
        </section>

        <section class="page-spread">
          <div class="page page-left">
            <span class="kicker">{{ page.label }}</span>
            <h2>{{ page.title }}</h2>
            <div class="story-copy">
              <p v-for="paragraph in page.left" :key="paragraph">{{ paragraph }}</p>
            </div>
          </div>

          <div class="page page-right">
            <span class="page-number">{{ String(currentPage + 1).padStart(2, "0") }}</span>
            <blockquote>{{ page.quote }}</blockquote>
            <div class="story-copy">
              <p v-for="paragraph in page.right" :key="paragraph">{{ paragraph }}</p>
            </div>
          </div>
        </section>
      </article>

      <section class="coming-soon" aria-label="Coming soon next chapters">
        <div class="coming-heading">
          <span>Coming Soon</span>
          <h2>Next Chapters</h2>
          <p>Perjalanan ini belum selesai. Bab-bab berikutnya akan membuka jarak, beban, dan hal-hal yang terlambat dijaga.</p>
        </div>

        <div class="coming-list">
          <article v-for="chapter in nextChapters" :key="chapter.title" class="coming-card">
            <span>{{ chapter.label }}</span>
            <h3>{{ chapter.title }}</h3>
            <p>{{ chapter.note }}</p>
          </article>
        </div>
      </section>
    </section>
  </main>
</template>
