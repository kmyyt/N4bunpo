[Uploading index.html…]()
<!DOCTYPE html>
<html lang="ko" class="light">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>JLPT N4 문법 스피드체크 웹 학습기 (001~114 완결판)</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script>
    tailwind.config = {
      darkMode: 'class',
      theme: {
        extend: {
          colors: {
            brand: {
              50: '#fff7ed',
              100: '#ffedd5',
              500: '#f97316',
              600: '#ea580c',
              700: '#c2410c',
            }
          }
        }
      }
    }
  </script>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=Noto+Sans+JP:wght@400;500;700&family=Noto+Sans+KR:wght@400;500;700&display=swap');
    body {
      font-family: 'Noto Sans KR', 'Noto Sans JP', sans-serif;
    }
    .jp-text {
      font-family: 'Noto Sans JP', sans-serif;
    }
    ruby rt {
      font-size: 0.65em;
      color: #ea580c;
      font-weight: 500;
    }
    .dark ruby rt {
      color: #fb923c;
    }
    .blur-target {
      transition: filter 0.2s ease, opacity 0.2s ease;
    }
    .memory-mode .blur-target {
      filter: blur(6px);
      user-select: none;
      cursor: pointer;
    }
    .memory-mode .blur-target:hover,
    .memory-mode .blur-target.revealed {
      filter: blur(0);
    }
  </style>
</head>
<body class="bg-slate-50 text-slate-800 dark:bg-slate-900 dark:text-slate-100 min-h-screen transition-colors duration-200">

  <!-- 상단 헤더 & 네비게이션 -->
  <header class="sticky top-0 z-50 bg-white/95 dark:bg-slate-800/95 backdrop-blur border-b border-slate-200 dark:border-slate-700 shadow-sm">
    <div class="max-w-6xl mx-auto px-4 py-3 flex flex-wrap items-center justify-between gap-3">
      <div class="flex items-center gap-2">
        <span class="bg-brand-500 text-white font-bold px-2.5 py-1 rounded-lg text-sm">JLPT N4</span>
        <h1 class="text-lg md:text-xl font-bold tracking-tight">스피드체크북 문법 마스터 (001~114)</h1>
      </div>

      <!-- 우측 컨트롤 버튼 (암기 모드 & 다크모드) -->
      <div class="flex items-center gap-2">
        <button id="memoryModeBtn" onclick="toggleMemoryMode()" class="flex items-center gap-1.5 px-3 py-1.5 rounded-lg text-sm font-medium bg-slate-100 hover:bg-slate-200 dark:bg-slate-700 dark:hover:bg-slate-600 transition">
          <span>🙈</span> <span id="memoryModeText">암기 모드 OFF</span>
        </button>
        <button id="darkModeBtn" onclick="toggleDarkMode()" class="p-2 rounded-lg bg-slate-100 hover:bg-slate-200 dark:bg-slate-700 dark:hover:bg-slate-600 transition" title="다크모드 전환">
          <span id="darkModeIcon">🌙</span>
        </button>
      </div>
    </div>

    <!-- 4개 메인 탭 -->
    <nav class="max-w-6xl mx-auto px-4 flex overflow-x-auto border-t border-slate-100 dark:border-slate-700/50">
      <button onclick="switchTab('dict')" id="tab-dict" class="tab-btn px-4 py-2.5 text-sm font-bold border-b-2 border-brand-500 text-brand-600 dark:text-brand-400 whitespace-nowrap">
        📖 문법 사전 (<span id="grammarTotalCount">114</span>)
      </button>
      <button onclick="switchTab('conj')" id="tab-conj" class="tab-btn px-4 py-2.5 text-sm font-medium border-b-2 border-transparent text-slate-500 hover:text-slate-800 dark:text-slate-400 dark:hover:text-slate-200 whitespace-nowrap">
        🔗 접속사 & 호응 부사
      </button>
      <button onclick="switchTab('tips')" id="tab-tips" class="tab-btn px-4 py-2.5 text-sm font-medium border-b-2 border-transparent text-slate-500 hover:text-slate-800 dark:text-slate-400 dark:hover:text-slate-200 whitespace-nowrap">
        💡 헷갈리는 문법 비교·팁
      </button>
      <button onclick="switchTab('quiz')" id="tab-quiz" class="tab-btn px-4 py-2.5 text-sm font-medium border-b-2 border-transparent text-slate-500 hover:text-slate-800 dark:text-slate-400 dark:hover:text-slate-200 whitespace-nowrap">
        ✍️ 실전 퀴즈 & 오답노트
      </button>
    </nav>
  </header>

  <!-- 메인 컨텐츠 영역 -->
  <main class="max-w-6xl mx-auto px-4 py-6">

    <!-- [탭 1] 문법 사전 -->
    <section id="section-dict" class="space-y-5">
      <!-- 검색 및 필터 바 -->
      <div class="bg-white dark:bg-slate-800 p-4 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700 space-y-3">
        <div class="flex flex-col md:flex-row gap-3">
          <div class="relative flex-1">
            <input type="text" id="searchInput" oninput="renderGrammarList()" placeholder="문법 번호(001~114), 일본어 표현, 한국어 뜻으로 검색..." class="w-full pl-10 pr-4 py-2 rounded-lg border border-slate-300 dark:border-slate-600 bg-slate-50 dark:bg-slate-900 focus:outline-none focus:ring-2 focus:ring-brand-500 text-sm">
            <span class="absolute left-3 top-2.5 text-slate-400">🔍</span>
          </div>
          <button id="bookmarkFilterBtn" onclick="toggleBookmarkFilter()" class="px-4 py-2 rounded-lg text-sm font-medium border border-slate-300 dark:border-slate-600 hover:bg-slate-100 dark:hover:bg-slate-700 transition flex items-center justify-center gap-1.5">
            <span>⭐</span> <span>북마크만 보기 (<span id="bookmarkCount">0</span>)</span>
          </button>
        </div>

        <!-- 카테고리 필터 버튼 -->
        <div class="flex flex-wrap gap-1.5" id="categoryButtons">
          <button onclick="setCategory('ALL')" data-cat="ALL" class="cat-btn px-3 py-1 rounded-full text-xs font-bold bg-brand-500 text-white">전체 (114)</button>
          <button onclick="setCategory('조사')" data-cat="조사" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">조사 (001~021)</button>
          <button onclick="setCategory('의지·희망')" data-cat="의지·희망" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">의지·희망·결정</button>
          <button onclick="setCategory('존경·겸양')" data-cat="존경·겸양" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">존경·겸양</button>
          <button onclick="setCategory('수수표현')" data-cat="수수표현" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">수수표현 (주다/받다)</button>
          <button onclick="setCategory('사역·수동')" data-cat="사역·수동" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">사역·수동·가능</button>
          <button onclick="setCategory('추측·전문')" data-cat="추측·전문" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">양태·추측·전문</button>
          <button onclick="setCategory('의무·허가')" data-cat="의무·허가" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">의무·허가·금지</button>
          <button onclick="setCategory('가정·조건')" data-cat="가정·조건" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">가정·조건</button>
          <button onclick="setCategory('일반문형')" data-cat="일반문형" class="cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200">기타 핵심문형</button>
        </div>
      </div>

      <!-- 문법 카드 리스트 그리드 -->
      <div id="grammarGrid" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
    </section>

    <!-- [탭 2] 필수 접속사 & 호응 부사 -->
    <section id="section-conj" class="hidden space-y-8">
      <div>
        <h2 class="text-xl font-bold mb-3 flex items-center gap-2">
          <span class="text-brand-500">■</span> 필수 접속사 CHECK!
        </h2>
        <div id="conjGrid" class="grid grid-cols-1 md:grid-cols-3 gap-4"></div>
      </div>

      <div>
        <h2 class="text-xl font-bold mb-3 flex items-center gap-2">
          <span class="text-brand-500">✓</span> 호응관계가 중요한 부사
        </h2>
        <div id="advGrid" class="grid grid-cols-1 md:grid-cols-2 gap-4"></div>
      </div>
    </section>

    <!-- [탭 3] 헷갈리는 문법 비교·암기 팁 -->
    <section id="section-tips" class="hidden space-y-6">
      <div class="bg-brand-50 dark:bg-slate-800/80 border border-brand-100 dark:border-slate-700 p-4 rounded-xl text-sm">
        💡 시험에 가장 자주 출제되는 <strong>유사 문법 비교 매트릭스</strong>입니다. 암기 모드를 켜고 핵심 차이점을 테스트해 보세요!
      </div>
      <div id="tipsContainer" class="space-y-6"></div>
    </section>

    <!-- [탭 4] 실전 퀴즈 & 오답 노트 -->
    <section id="section-quiz" class="hidden max-w-3xl mx-auto space-y-6">
      <div class="bg-white dark:bg-slate-800 p-6 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700">
        <div class="flex items-center justify-between mb-4">
          <span id="quizModeBadge" class="px-3 py-1 rounded-full text-xs font-bold bg-brand-100 text-brand-700 dark:bg-brand-700 dark:text-white">전체 실전 퀴즈</span>
          <span id="quizProgress" class="text-sm font-medium text-slate-500 dark:text-slate-400">문제 1 / 10</span>
        </div>

        <div id="quizQuestionBox" class="space-y-5">
          <div class="flex items-start justify-between gap-2">
            <h3 id="quizQuestion" class="text-lg md:text-xl font-bold jp-text leading-relaxed"></h3>
            <button id="quizTtsBtn" class="p-2 rounded-lg bg-slate-100 hover:bg-slate-200 dark:bg-slate-700 text-sm shrink-0" title="문제 듣기">🔊</button>
          </div>
          <p id="quizKoHint" class="text-sm text-slate-500 dark:text-slate-400"></p>
          <div id="quizOptions" class="grid grid-cols-1 gap-2.5"></div>
          <div id="quizFeedback" class="hidden p-4 rounded-lg text-sm space-y-1"></div>
          <div class="flex justify-end pt-2">
            <button id="nextQuizBtn" onclick="nextQuestion()" class="hidden px-5 py-2.5 rounded-lg bg-brand-500 hover:bg-brand-600 text-white font-bold text-sm transition">다음 문제 ➔</button>
          </div>
        </div>

        <!-- 퀴즈 결과 화면 -->
        <div id="quizResultBox" class="hidden text-center py-8 space-y-4">
          <div class="text-4xl">🎉</div>
          <h3 class="text-2xl font-bold">퀴즈 완료!</h3>
          <p id="quizScoreText" class="text-lg text-slate-600 dark:text-slate-300"></p>
          <div class="flex justify-center gap-3 pt-4">
            <button onclick="startQuiz(false)" class="px-5 py-2.5 rounded-lg bg-slate-200 hover:bg-slate-300 dark:bg-slate-700 dark:hover:bg-slate-600 font-bold text-sm transition">전체 다시 풀기</button>
            <button id="retryWrongBtn" onclick="startQuiz(true)" class="hidden px-5 py-2.5 rounded-lg bg-brand-500 hover:bg-brand-600 text-white font-bold text-sm transition">오답만 다시 풀기 (<span id="wrongCount">0</span>)</button>
          </div>
        </div>
      </div>
    </section>

  </main>

  <script>
    // ==========================================
    // 1. 필수 접속사 & 호응 부사 데이터
    // ==========================================
    const conjunctionsData = [
      {
        type: "순접 (그리하여 / 그러니까)",
        items: [
          { jp: "だから・ですから", ko: "그러니까, 그래서 (원인 ➔ 결과)", ex: "あめが ふっている。だから いかない。" },
          { jp: "それで・そこで", ko: "그래서, 그리하여 (앞 상황을 받아 행동)", ex: "さいふを わすれた。そこで 友だちに かりた。" },
          { jp: "では・それでは", ko: "그럼, 그렇다면 (화제 전환/결론)", ex: "では、つぎの もんだいに いきます。" },
          { jp: "それなら", ko: "그렇다면 (상대방 말을 받아 조건 제시)", ex: "ひまですか。それなら 手つだって ください。" },
          { jp: "すると", ko: "그러자, 그랬더니 (직후에 일어난 일)", ex: "まどを あけた。すると すずしい かぜが 入ってきた。" }
        ]
      },
      {
        type: "역접 (그러나 / 하지만)",
        items: [
          { jp: "でも", ko: "하지만, 그렇지만 (회화체)", ex: "たかい。でも かいたい。" },
          { jp: "しかし", ko: "그러나, 하지만 (문어체/격식)", ex: "べんきょうした。しかし しけんに おちた。" },
          { jp: "だが", ko: "하지만, 그러나 (문어체)", ex: "かれは わかい。だが けいけんが おおい。" },
          { jp: "けれども", ko: "하지만, ~이지만", ex: "いそがしいけれども、たのしいです。" }
        ]
      },
      {
        type: "첨가 · 선택 · 설명",
        items: [
          { jp: "そして (첨가)", ko: "그리고, 그러고 나서", ex: "ほんを よみ、そして ねました。" },
          { jp: "それに (첨가)", ko: "게다가, 더구나", ex: "ここは しずかだ。それに こうつうも べんりだ。" },
          { jp: "それから (첨가)", ko: "그리고 나서, 그 후", ex: "まず てを あらい、それから ごはんを たべる。" },
          { jp: "または / それとも (선택)", ko: "또는 / 혹은 (~인가요, 아니면 ~인가요?)", ex: "コーヒーにしますか。それとも こうちゃにしますか。" },
          { jp: "なぜなら (설명)", ko: "왜냐하면 (~からだ 와 호응)", ex: "やすみました。なぜなら かぜを ひいたからです。" }
        ]
      }
    ];

    const adverbsData = [
      {
        category: "1. 부사 + 부정어 (~ない)",
        items: [
          { jp: "けっして + ~ない", ko: "결코 ~하지 않다", ex: "けっして うそは つきません。" },
          { jp: "ぜんぜん + ~ない", ko: "전혀 ~하지 않다", ex: "えいごが ぜんぜん わかりません。" },
          { jp: "なかなか + ~ない", ko: "좀처럼 ~하지 않다", ex: "バスが なかなか 来(き)ません。" },
          { jp: "なにも / だれも / どこも + ~ない", ko: "아무것도 / 아무도 / 아무데도 ~않다", ex: "へやには だれも いません。" },
          { jp: "あまり + ~ない", ko: "그다지(별로) ~하지 않다", ex: "おさけは あまり のみません。" }
        ]
      },
      {
        category: "2. 부사 + 희망·권유 / 추량 / 가정",
        items: [
          { jp: "ぜひ + ~たい / ~てください", ko: "꼭 ~하고 싶다 / 꼭 ~해 주세요 (희망·권유)", ex: "ぜひ あそびに 来(き)て ください。" },
          { jp: "どうぞ + ~てください", ko: "아무쪼록(부디) ~해 주세요 (권유)", ex: "どうぞ たくさん たべて ください。" },
          { jp: "たぶん + ~だろう", ko: "아마 ~일 것이다 (추량)", ex: "あしたは たぶん いい 天気(てんき)だろう。" },
          { jp: "きっと + ~でしょう", ko: "틀림없이(분명) ~일 것입니다 (추량)", ex: "かれは きっと 来(く)るでしょう。" },
          { jp: "もし + ~たら", ko: "만약 ~라면 (가정)", ex: "もし あめが ふったら、いきません。" }
        ]
      }
    ];

    // ==========================================
    // 2. 헷갈리는 문법 비교·암기 팁 데이터
    // ==========================================
    const comparisonTips = [
      {
        title: "1) 한정 표현 비교: ~しか vs ~だけ vs ~ばかり",
        headers: ["구분", "~しか (006)", "~だけ (007)", "~ばかり (018)"],
        rows: [
          ["핵심 뜻", "~밖에 (부정적 강조)", "~만, ~뿐 (객관적 한정)", "~만, ~뿐 (반복/편중)"],
          ["호응 서술어", "무조건 부정문 (~ない)", "주로 긍정문 (부정도 가능)", "긍정문 (동사 て형/명사 접속)"],
          ["예문 비교", "千円しか ない (천 엔밖에 없다)", "やさしい ことばだけ 覚えた (쉬운 말만 외웠다)", "はたらいてばかり いる (일만 하고 있다)"]
        ]
      },
      {
        title: "2) 이유 표현 비교: ~から vs ~ので vs ~ため(に)",
        headers: ["구분", "~から (003)", "~ので (015)", "~ため(に) (056)"],
        rows: [
          ["뉘앙스/특징", "주관적 이유 (명령·권유·의지문 가능)", "객관적·정중한 이유 (부탁·사과에 자주 사용)", "원인·이유 (문어체) / ~하기 위해 (목적)"],
          ["な형용사·명사 접속", "だ + から (しけんだから)", "な + ので (しけんなので)", "の + ために (びょうきのため)"],
          ["예문", "つかれたから、休みましょう", "しずかできれいなので、さんぽに行く", "びょうきのため、たばこをやめます"]
        ]
      },
      {
        title: "3) 양태 vs 전문 vs 추량: ~そうだ / ~ようだ / ~らしい",
        headers: ["구분", "양태 ~そうだ (050)", "전문 ~そうだ (050)", "추량 ~ようだ (107) / ~らしい (111)"],
        rows: [
          ["핵심 의미", "~할 것 같다, ~해 보인다 (시각적 직감)", "~라고 한다 (들은 소식 전달)", "~인 것 같다 (상황 판단 / 객관적 근거)"],
          ["접속 형태", "동사 ます형 / 형용사 어간 + そうだ", "동사·형용사·명사 보통형 + そうだ", "보통형(명사の/な형용사な)+ようだ / 보통형+らしい"],
          ["예문", "おいしそうに 飲んでいる (맛있어 보이게)", "じしんが あったそうです (있었다고 합니다)", "きのうより さむいようです (추운 것 같습니다)"]
        ]
      },
      {
        title: "4) 상태 표현 비교: 자동사 + ~ている vs 타동사 + ~てある (067)",
        headers: ["구분", "자동사 + ~ている (067)", "타동사 + ~てある (067)", "~ておく (069)"],
        rows: [
          ["핵심 뉘앙스", "자연스러운 결과 상태 (~되어 있다)", "누군가 의도를 갖고 해 놓은 상태 (~해져 있다)", "다음 일을 위해 미리 '~해 두다'"],
          ["앞에 오는 조사", "대상 + が + 자동사 ている", "대상 + が(또는 を) + 타동사 てある", "대상 + を + 타동사 ておく"],
          ["예문", "まどが しまって います (창문이 닫혀 있다)", "スプーンと はしが ならべて あります (놓여 있다)", "よく 覚えて おいて ください (기억해 두세요)"]
        ]
      },
      {
        title: "5) 수수표현(주고받기) 7종 완벽 정리 (064, 066, 071, 073, 080, 081)",
        headers: ["방향 / 관계", "윗사람 (존경/겸양)", "동등·일반", "아랫사람·동식물"],
        rows: [
          ["내가(제3자가) 남에게 주다", "~てさしあげる (겸양: 해 드리다)", "~てあげる (064: 해 주다)", "~てやる (081: 해 주다)"],
          ["남이 나에게(우리에게) 주다", "~てくださる (071 존경: 해 주시다)", "~てくれる (073: 해 주다)", "-"],
          ["내가 남에게서 받다", "~ていただく (066 겸양: 해 받다)", "~てもらう (080: 해 받다)", "-"]
        ]
      },
      {
        title: "6) 시점 표현 비교: ~ところだ 3총사 (054, 068, 085)",
        headers: ["구분", "사전형 + ところだ (085)", "ている + ところだ (068)", "た(과거형) + ところだ (054)"],
        rows: [
          ["시점 의미", "막 ~하려는 참이다 (동작 직전)", "지금 ~하고 있는 중이다 (동작 진행)", "방금 막 ~한 참이다 (동작 직후)"],
          ["호응 부사", "今から・これから (지금부터/이제부터)", "今 (지금)", "たった今・ちょうど今 (방금 막)"],
          ["예문", "今から 本を かえしに 行くところです", "今 れきしの 本を 読んでいるところです", "かいぎは 今 終わったところです"]
        ]
      }
    ];

    // ==========================================
    // 3. 문법 사전 데이터 (001 ~ 114번 완결)
    // ==========================================
    const grammarData = [
      {
        id: "001", title: "~か / ~か~ないか", mean: "~인가, ~인지 / ~할지 ~하지 않을지", cat: "조사",
        conn: "의문사 + か / 동사 사전형 + か + 동사 ない형 + か",
        point: "의문사+か+知っていますか/分かりますか 패턴과 선택(~이나: あしたか あさって) 용법 주의!",
        ex: [
          { jp: "パーティーは <ruby>何時<rt>なんじ</rt></ruby>に <ruby>始<rt>はじ</rt></ruby>まるか <ruby>知<rt>し</rt></ruby>って いますか。", tts: "パーティーは何時に始まるか知っていますか。", ko: "파티는 몇 시에 시작되는지 알고 있습니까?" },
          { jp: "<ruby>家<rt>いえ</rt></ruby>を <ruby>出<rt>で</rt></ruby>るか <ruby>出<rt>で</rt></ruby>ないかという ときに <ruby>雨<rt>あめ</rt></ruby>が <ruby>降<rt>ふ</rt></ruby>り<ruby>出<rt>だ</rt></ruby>した。", tts: "家を出るか出ないかというときに雨が降り出した。", ko: "집을 나설까 말까 하고 있을 때 비가 내리기 시작했다." }
        ]
      },
      {
        id: "002", title: "~が", mean: "~이/가 (주격·대상)", cat: "조사",
        conn: "명사 + が",
        point: "목적격 조사 を 대신 が를 쓰는 표현: 好きだ・嫌いだ・上手だ・下手だ・欲しい・分かる・できる(가능동사)",
        ex: [
          { jp: "<ruby>友<rt>とも</rt></ruby>だちが <ruby>日本<rt>にほん</rt></ruby>の おもちゃを おくって くれた。", tts: "友だちが日本のおもちゃをおくってくれた。", ko: "친구가 일본 장난감을 보내 주었다." },
          { jp: "わたしは <ruby>日本語<rt>にほんご</rt></ruby>が <ruby>少<rt>すこ</rt></ruby>し <ruby>話<rt>はな</rt></ruby>せます。", tts: "わたしは日本語が少し話せます。", ko: "나는 일본어를 조금 할 줄 압니다." }
        ]
      },
      {
        id: "003", title: "~から", mean: "~기 때문에, ~으로(원료), ~로부터", cat: "조사",
        conn: "보통형/명사 + から",
        point: "원인·이유 / 화학적 변화의 원료(우유로 치즈 등) / 동작의 출처(~로부터)",
        ex: [
          { jp: "だいぶ つかれたから、ちょっと <ruby>休<rt>やす</rt></ruby>みましょう。", tts: "だいぶつかれたから、ちょっと休みましょう。", ko: "몹시 피곤하니까 잠시 쉽시다." },
          { jp: "バターや チーズは <ruby>牛乳<rt>ぎゅうにゅう</rt></ruby>から <ruby>作<rt>つく</rt></ruby>ります。", tts: "バターやチーズは牛乳から作ります。", ko: "버터랑 치즈는 우유로 만듭니다." }
        ]
      },
      {
        id: "004", title: "~ぐらい", mean: "~정도, ~쯤", cat: "조사",
        conn: "수량사·명사 + ぐらい (유사표현: ~くらい)",
        point: "대략적인 수량이나 정도를 나타낼 때 사용",
        ex: [
          { jp: "<ruby>日本語<rt>にほんご</rt></ruby>の <ruby>本<rt>ほん</rt></ruby>を 50さつぐらい <ruby>持<rt>も</rt></ruby>って います。", tts: "日本語の本を50さつぐらい持っています。", ko: "일본어 책을 50권 정도 갖고 있습니다." },
          { jp: "<ruby>駅<rt>えき</rt></ruby>まで 10<ruby>分<rt>ぷん</rt></ruby>ぐらい かかります。", tts: "駅まで10分ぐらいかかります。", ko: "역까지 10분 정도 걸립니다." }
        ]
      },
      {
        id: "005", title: "~けれど(も)・けど", mean: "~하지만, ~할 텐데", cat: "조사",
        conn: "문장(보통형/정중형) + けれど(も)・けど",
        point: "역접 또는 앞말을 부드럽게 이어주는 접속조사 (유사표현: ~が 문어체)",
        ex: [
          { jp: "<ruby>前<rt>まえ</rt></ruby>に <ruby>読<rt>よ</rt></ruby>んだ ことは あるけれど、よく <ruby>覚<rt>おぼ</rt></ruby>えて いませんね。", tts: "前に読んだことはあるけれど、よく覚えていませんね。", ko: "전에 읽은 적은 있지만 잘 기억이 안 나는군요." },
          { jp: "あした デパートへ <ruby>行<rt>い</rt></ruby>くけど、いっしょに <ruby>行<rt>い</rt></ruby>きませんか。", tts: "あしたデパートへ行くけど、いっしょに行きませんか。", ko: "내일 백화점에 가는데, 같이 갈래요?" }
        ]
      },
      {
        id: "006", title: "~しか", mean: "~밖에", cat: "조사",
        conn: "명사 + しか + 부정문(~ない)",
        point: "반드시 부정문에서만 사용! (~だけ는 긍정문에서 사용)",
        ex: [
          { jp: "<ruby>今<rt>いま</rt></ruby> <ruby>持<rt>も</rt></ruby>っている おかねは <ruby>千円<rt>せんえん</rt></ruby>くらいしか ない。", tts: "今持っているおかねは千円くらいしかない。", ko: "지금 갖고 있는 돈은 천 엔 정도밖에 없다." },
          { jp: "ひらがなしか <ruby>書<rt>か</rt></ruby>けません。", tts: "ひらがなしか書けません。", ko: "히라가나밖에 쓸 수 없습니다." }
        ]
      },
      {
        id: "007", title: "~だけ", mean: "~만, ~뿐, 딱~", cat: "조사",
        conn: "명사·동사·형용사 + だけ",
        point: "주로 긍정문에서 한정을 나타냄",
        ex: [
          { jp: "やさしい ことばだけ <ruby>覚<rt>おぼ</rt></ruby>えました。", tts: "やさしいことばだけ覚えました。", ko: "쉬운 단어만 외웠습니다." },
          { jp: "あと <ruby>少<rt>すこ</rt></ruby>しだけ <ruby>待<rt>ま</rt></ruby>って ください。", tts: "あと少しだけ待ってください。", ko: "앞으로 조금만 더 기다려 주세요." }
        ]
      },
      {
        id: "008", title: "~で", mean: "~으로(수단), ~때문에(이유), ~에서(장소), ~만에", cat: "조사",
        conn: "명사 + で",
        point: "수단·방법·도구 / 원인·이유 / 시간·수량 한계(3分でできます) / 행위의 장소(教室で勉強する)",
        ex: [
          { jp: "じしんで <ruby>建物<rt>たてもの</rt></ruby>が たおれました。", tts: "じしんで建物がたおれました。", ko: "지진 때문에 건물이 무너졌습니다." },
          { jp: "<ruby>日本語<rt>にほんご</rt></ruby>が わからないので <ruby>英語<rt>えいご</rt></ruby>で <ruby>話<rt>はな</rt></ruby>しました。", tts: "日本語がわからないので英語で話しました。", ko: "일본어를 모르기 때문에 영어로 말했습니다." }
        ]
      },
      {
        id: "009", title: "~でも", mean: "~(이)라도, ~든지", cat: "조사",
        conn: "명사 + でも / 의문사 + でも",
        point: "가벼운 예시(커피라도) / 의문사+でも(전면적 긍정: いつでも・何でも・どこでも・だれでも)",
        ex: [
          { jp: "つかれたから、コーヒーでも <ruby>飲<rt>の</rt></ruby>みましょう。", tts: "つかれたから、コーヒーでも飲みましょう。", ko: "피곤하니까 커피라도 마십시다." },
          { jp: "あなたの ためなら、どんな ことでも します。", tts: "あなたのためなら、どんなことでもします。", ko: "당신을 위해서라면 어떤 일이라도 하겠습니다." }
        ]
      },
      {
        id: "010", title: "~と / ~って / ~という", mean: "~와/과, ~라고, ~라고 하는", cat: "조사",
        conn: "명사 + と / 인용문 + と(って)",
        point: "~と(명사 대등 접속, 공동 동작) / ~と・~って(인용) / ~という・~っていう(~라는)",
        ex: [
          { jp: "あそこに 「<ruby>入<rt>い</rt></ruby>り<ruby>口<rt>ぐち</rt></ruby>」と <ruby>書<rt>か</rt></ruby>いて あります。", tts: "あそこに「入り口」と書いてあります。", ko: "저기에 '입구'라고 적혀 있습니다." },
          { jp: "これは たこ<ruby>焼<rt>や</rt></ruby>きという <ruby>食<rt>た</rt></ruby>べ<ruby>物<rt>もの</rt></ruby>です。", tts: "これはたこ焼きという食べ物です。", ko: "이것은 타코야키라고 하는 음식입니다." }
        ]
      },
      {
        id: "011", title: "~とか", mean: "~이나, ~든지 (예시 나열)", cat: "조사",
        conn: "명사·동사 사전형 + とか",
        point: "유사표현: ~や ~など (本やノートなどを買いました)",
        ex: [
          { jp: "<ruby>病気<rt>びょうき</rt></ruby>の おみまいには くだものとか お<ruby>花<rt>はな</rt></ruby>とかが いいです。", tts: "病気のおみまいにはくだものとかお花とかがいいです。", ko: "병문안을 갈 때는 과일이나 꽃이 좋습니다." },
          { jp: "<ruby>休<rt>やす</rt></ruby>みの <ruby>日<rt>ひ</rt></ruby>は テレビを <ruby>見<rt>み</rt></ruby>るとか して すごします。", tts: "休みの日はテレビを見るとかしてすごします。", ko: "쉬는 날에는 TV를 본다든지 하며 보냅니다." }
        ]
      },
      {
        id: "012", title: "~な", mean: "~지 마라 (금지)", cat: "조사",
        conn: "동사 사전형 + な",
        point: "강한 금지 명령을 나타내는 종조사",
        ex: [
          { jp: "そんな きたない くつ<ruby>下<rt>した</rt></ruby>、はくなよ。", tts: "そんなきたないくつ下、はくなよ。", ko: "그런 더러운 양말 신지 마라." },
          { jp: "あぶないから、そとへ <ruby>出<rt>で</rt></ruby>るな。", tts: "あぶないから、そとへ出るな。", ko: "위험하니까 밖으로 나가지 마라." }
        ]
      },
      {
        id: "013", title: "~に", mean: "~에, ~에게, ~하러(목적)", cat: "조사",
        conn: "명사 / 동사 ます형 + に",
        point: "함께 쓰는 필수 동사: ~に乗る・会う・勝つ・似る・住む・遅れる・気が付く・気を付ける・泊まる",
        ex: [
          { jp: "わたしは <ruby>子<rt>こ</rt></ruby>どもの ころ、よく <ruby>母<rt>はは</rt></ruby>に おこられました。", tts: "わたしは子どものころ、よく母におこられました。", ko: "나는 어렸을 적 종종 어머니에게 야단맞았습니다. (수동문 동작주)" },
          { jp: "1<ruby>年<rt>ねん</rt></ruby>に 3<ruby>回<rt>かい</rt></ruby>、かぞくと <ruby>食事<rt>しょくじ</rt></ruby>に <ruby>行<rt>い</rt></ruby>きます。", tts: "1年に3回、かぞくと食事に行きます。", ko: "1년에 3번(빈도), 가족과 식사하러 갑니다(목적)." }
        ]
      },
      {
        id: "014", title: "~の", mean: "~것(형식명사), ~거야?(종조사)", cat: "조사",
        conn: "동사·형용사 보통형 + の",
        point: "문장의 명사화: ~のが見える・聞こえる・見る / 의문·이유를 묻는 종조사 の?",
        ex: [
          { jp: "<ruby>山田<rt>やまだ</rt></ruby>さんが <ruby>歌<rt>うた</rt></ruby>って いるのが <ruby>聞<rt>き</rt></ruby>こえます。", tts: "山田さんが歌っているのが聞こえます。", ko: "야마다 씨가 노래를 부르고 있는 것이 들립니다." },
          { jp: "どうして きのうは かいしゃを <ruby>休<rt>やす</rt></ruby>んだの？", tts: "どうしてきのうはかいしゃを休んだの？", ko: "어제는 왜 회사를 쉰 거야?" }
        ]
      },
      {
        id: "015", title: "~ので", mean: "~이므로, ~이기 때문에", cat: "조사",
        conn: "동사·い형용사 보통형 + ので / な형용사·명사 + なので",
        point: "な형용사·명사 뒤에 なので가 오는 형태 주의! (きれいなので・簡単なので・しけんなので)",
        ex: [
          { jp: "その <ruby>公園<rt>こうえん</rt></ruby>は しずかで きれいなので、よく さんぽに <ruby>行<rt>い</rt></ruby>きます。", tts: "その公園はしずかできれいなので、よくさんぽに行きます。", ko: "그 공원은 조용하고 깨끗하기 때문에 자주 산책하러 갑니다." },
          { jp: "あしたは しけんなので、はやく <ruby>寝<rt>ね</rt></ruby>ます。", tts: "あしたはしけんなので、はやく寝ます。", ko: "내일은 시험이므로 일찍 잡니다." }
        ]
      },
      {
        id: "016", title: "~のに", mean: "~인데도(역접), ~하는 데(목적·용도)", cat: "조사",
        conn: "보통형 + のに (な형용사·명사 + なのに) / 동사 사전형 + のに",
        point: "역접일 때는 명사/な형용사+なのに, 용도·목적(~하는 데 걸리다/쓰다)일 때는 동사 사전형+のに",
        ex: [
          { jp: "かれは <ruby>子<rt>こ</rt></ruby>どもなのに タバコを すって いる。", tts: "かれは子どもなのにタバコをすっている。", ko: "그는 어린이인데도 담배를 피우고 있다." },
          { jp: "この はしを つくるのに 4<ruby>年<rt>ねん</rt></ruby> かかりました。", tts: "このはしをつくるのに4年かかりました。", ko: "이 다리를 만드는 데 4년 걸렸습니다." }
        ]
      },
      {
        id: "017", title: "~は", mean: "~은/는 (주제·대조)", cat: "조사",
        conn: "명사 + は",
        point: "문장의 주제 제시 또는 다른 것과 대조·강조할 때 사용",
        ex: [
          { jp: "わたしは おさけは <ruby>飲<rt>の</rt></ruby>みません。", tts: "わたしはおさけは飲みません。", ko: "나는 술은 안 마십니다." },
          { jp: "きのうは <ruby>雨<rt>あめ</rt></ruby>でしたが、きょうは いい <ruby>天気<rt>てんき</rt></ruby>です。", tts: "きのうは雨でしたが、きょうはいい天気です。", ko: "어제는 비가 왔지만, 오늘은 좋은 날씨입니다." }
        ]
      },
      {
        id: "018", title: "~ばかり", mean: "~만, ~뿐", cat: "조사",
        conn: "동사 て형·명사 + ばかり",
        point: "특정 행동이나 사물만 반복·편중됨을 나타냄 (비교표현: ~だけ)",
        ex: [
          { jp: "お<ruby>父<rt>とう</rt></ruby>さんは はたらいてばかりで、むすこと あそぶ <ruby>時間<rt>じかん</rt></ruby>が ない。", tts: "お父さんははたらいてばかりで、むすことあそぶ時間がない。", ko: "아버지는 일만 해서 아들과 놀 시간이 없다." },
          { jp: "あまい ものばかり <ruby>食<rt>た</rt></ruby>べないで ください。", tts: "あまいものばかり食べないでください。", ko: "단 것만 먹지 마세요." }
        ]
      },
      {
        id: "019", title: "~までに", mean: "~까지(는) (기한·마감 시점)", cat: "조사",
        conn: "시간 명사 + までに",
        point: "まで는 계속되는 동작의 끝(선), までに는 일회성 동작이 완료되어야 하는 마감 시점(점)!",
        ex: [
          { jp: "あしたは あさ 9<ruby>時<rt>じ</rt></ruby>までに わたしの ところに <ruby>来<rt>き</rt></ruby>て ください。", tts: "あしたはあさ9時までにわたしのところに来てください。", ko: "내일은 아침 아홉 시까지 나한테 와 주세요." },
          { jp: "<ruby>金曜日<rt>きんようび</rt></ruby>までに この レポートを <ruby>出<rt>だ</rt></ruby>して ください。", tts: "金曜日までにこのレポートを出してください。", ko: "금요일까지 이 리포트를 제출해 주세요." }
        ]
      },
      {
        id: "020", title: "~も / ~も~も", mean: "~도, ~(이)나 / ~도 ~도", cat: "조사",
        conn: "조수사 + も / 명사 + も + 명사 + も",
        point: "수량사 뒤에 も가 붙으면 생각보다 많음(~이나)을 강조!",
        ex: [
          { jp: "<ruby>駅<rt>えき</rt></ruby>から <ruby>学校<rt>がっこう</rt></ruby>まで あるいて 20<ruby>分<rt>ぷん</rt></ruby>も かかります。", tts: "駅から学校まであるいて20分もかかります。", ko: "역에서 학교까지 걸어서 20분이나 걸립니다." },
          { jp: "わたしの <ruby>父<rt>ちち</rt></ruby>は <ruby>英語<rt>えいご</rt></ruby>も <ruby>日本語<rt>にほんご</rt></ruby>も <ruby>話<rt>はな</rt></ruby>せます。", tts: "わたしの父は英語も日本語も話せます。", ko: "우리 아빠는 영어도 일본어도 할 줄 압니다." }
        ]
      },
      {
        id: "021", title: "~を", mean: "~을/를 (대상·기점·경유지)", cat: "조사",
        conn: "명사 + を",
        point: "기점·경로·경유지 동사: 角を曲がる・公園を歩く/走る・空を飛ぶ・道を通る・橋を渡る",
        ex: [
          { jp: "けさ、<ruby>電車<rt>でんしゃ</rt></ruby>の <ruby>中<rt>なか</rt></ruby>で <ruby>足<rt>あし</rt></ruby>を ふまれました。", tts: "けさ、電車の中で足をふまれました。", ko: "오늘 아침 전철 안에서 발을 밟혔습니다. (수동문 대상)" },
          { jp: "<ruby>両親<rt>りょうしん</rt></ruby>を <ruby>安心<rt>あんしん</rt></ruby>させるために <ruby>電話<rt>でんわ</rt></ruby>しました。", tts: "両親を安心させるために電話しました。", ko: "부모님을 안심시키기 위해 전화했습니다. (사역문 대상)" }
        ]
      },
      {
        id: "022", title: "~あいだ(間)", mean: "~하는 동안에, 사이에", cat: "일반문형",
        conn: "동사 ている・명사の + あいだ",
        point: "あいだ는 기간 내내 계속되는 동작, あいだに는 그 기간 중 어느 한 시점에 일어나는 동작!",
        ex: [
          { jp: "じゅぎょうの <ruby>間<rt>あいだ</rt></ruby>は しずかに して ください。", tts: "じゅぎょうの間はしずかにしてください。", ko: "수업 동안에는 조용히 해 주세요." },
          { jp: "<ruby>母<rt>はは</rt></ruby>が <ruby>寝<rt>ね</rt></ruby>て いる <ruby>間<rt>あいだ</rt></ruby>、わたしは <ruby>本<rt>ほん</rt></ruby>を <ruby>読<rt>よ</rt></ruby>んで いた。", tts: "母が寝ている間、わたしは本を読んでいた。", ko: "어머니가 주무시는 동안 나는 책을 읽고 있었다." }
        ]
      },
      {
        id: "023", title: "いくら~ても", mean: "아무리 ~해도", cat: "가정·조건",
        conn: "いくら + 동사·형용사 て형(명사·な형용사 + でも)",
        point: "유사표현: どんなに~ても(でも) (087번)",
        ex: [
          { jp: "あの <ruby>人<rt>ひと</rt></ruby>の <ruby>話<rt>はなし</rt></ruby>は いくら <ruby>聞<rt>き</rt></ruby>いても わかりません。", tts: "あの人の話はいくら聞いてもわかりません。", ko: "저 사람의 이야기는 아무리 들어도 모르겠습니다." },
          { jp: "いくら <ruby>安<rt>やす</rt></ruby>くても、いらない ものは <ruby>買<rt>か</rt></ruby>いません。", tts: "いくら安くても、いらないものは買いません。", ko: "아무리 싸더라도 필요 없는 것은 사지 않습니다." }
        ]
      },
      {
        id: "024", title: "~(よ)う / ~(よ)うか", mean: "~하겠다, ~하자, ~할게 / ~할까", cat: "의지·희망",
        conn: "동사 의지형 + (よ)う・(よ)うか",
        point: "정중형: ~ましょう(~합시다) / ~ましょうか(~할까요?)",
        ex: [
          { jp: "わたしが やろう。", tts: "わたしがやろう。", ko: "내가 할게." },
          { jp: "お<ruby>茶<rt>ちゃ</rt></ruby>を <ruby>入<rt>い</rt></ruby>れようか。", tts: "お茶を入れようか。", ko: "차를 끓일까?" }
        ]
      },
      {
        id: "025", title: "~(よ)うと思う", mean: "~하려고 생각하다", cat: "의지·희망",
        conn: "동사 의지형 + (よ)うと思う",
        point: "화자의 결심이나 의지를 나타냄 (~ようと思っています는 이전부터 계속 생각 중인 의지)",
        ex: [
          { jp: "<ruby>毎日<rt>まいにち</rt></ruby> <ruby>少<rt>すこ</rt></ruby>しずつ <ruby>勉強<rt>べんきょう</rt></ruby>しようと <ruby>思<rt>おも</rt></ruby>って います。", tts: "毎日少しずつ勉強しようと思っています。", ko: "매일 조금씩 공부하려고 생각하고 있습니다." },
          { jp: "らいねん <ruby>車<rt>くるま</rt></ruby>を <ruby>買<rt>か</rt></ruby>おうと <ruby>思<rt>おも</rt></ruby>います。", tts: "らいねん車を買おうと思います。", ko: "내년에 차를 사려고 생각합니다." }
        ]
      },
      {
        id: "026", title: "~(よ)うとする", mean: "~하려고 하다 (직전 동작·시도)", cat: "의지·희망",
        conn: "동사 의지형 + (よ)うとする",
        point: "막 어떤 동작을 하려는 순간이나 시도할 때 자주 사용 (~ようとしたとき)",
        ex: [
          { jp: "<ruby>出<rt>で</rt></ruby>かけようと した とき、<ruby>電話<rt>でんわ</rt></ruby>が かかって きました。", tts: "出かけようとしたとき、電話がかかってきました。", ko: "외출하려고 했을 때 전화가 걸려 왔습니다." },
          { jp: "<ruby>電車<rt>でんしゃ</rt></ruby>に <ruby>乗<rt>の</rt></ruby>ろうと したら、ドアが しまった。", tts: "電車に乗ろうとしたら、ドアがしまった。", ko: "전철에 타려고 했더니 문이 닫혔다." }
        ]
      },
      {
        id: "027", title: "お~ください", mean: "~해 주세요 (존경 정중 요청)", cat: "존경·겸양",
        conn: "お + 동사 ます형 + ください",
        point: "~てください의 존경어로 손님이나 윗사람에게 정중히 안내·요청할 때 사용",
        ex: [
          { jp: "この <ruby>紙<rt>かみ</rt></ruby>に <ruby>住所<rt>じゅうしょ</rt></ruby>と お<ruby>名前<rt>なまえ</rt></ruby>を お<ruby>書<rt>か</rt></ruby>き ください。", tts: "この紙に住所とお名前をお書きください。", ko: "이 종이에 주소와 성함을 적어 주세요." },
          { jp: "どうぞ こちらへ お<ruby>入<rt>はい</rt></ruby>り ください。", tts: "どうぞこちらへお入りください。", ko: "부디 이쪽으로 들어오십시오." }
        ]
      },
      {
        id: "028", title: "お~する", mean: "~하다 / ~해 드리다 (겸양어)", cat: "존경·겸양",
        conn: "お + 동사 ます형 + する(いたす)",
        point: "내 행동을 낮추어 상대방을 높이는 겸양 표현",
        ex: [
          { jp: "この <ruby>品物<rt>しなもの</rt></ruby>は、きれいな かみで おつつみします。", tts: "この品物は、きれいなかみでおつつみします。", ko: "이 상품은 예쁜 종이로 포장해 드리겠습니다." },
          { jp: "<ruby>重<rt>おも</rt></ruby>そうですから、かばんを お<ruby>持<rt>も</rt></ruby>ちしましょう。", tts: "重そうですから、かばんをお持ちしましょう。", ko: "무거워 보이니 가방을 들어 드리겠습니다." }
        ]
      },
      {
        id: "029", title: "お~になる", mean: "~하시다 (존경어)", cat: "존경·겸양",
        conn: "お + 동사 ます형 + になる",
        point: "윗사람의 행동을 높이는 대표적인 존경 표현",
        ex: [
          { jp: "<ruby>社長<rt>しゃちょう</rt></ruby>は、いま <ruby>新聞<rt>しんぶん</rt></ruby>を お<ruby>読<rt>よ</rt></ruby>みに なって います。", tts: "社長は、いま新聞をお読みになっています。", ko: "사장님은 지금 신문을 읽고 계십니다." },
          { jp: "<ruby>先生<rt>せんせい</rt></ruby>は もう お<ruby>帰<rt>かえ</rt></ruby>りに なりました。", tts: "先生はもうお帰りになりました。", ko: "선생님은 벌써 돌아가셨습니다." }
        ]
      },
      {
        id: "030", title: "~おわる", mean: "다 ~하다, ~하기를 끝내다", cat: "일반문형",
        conn: "동사 ます형 + おわる",
        point: "동작이 완전히 끝남을 나타내는 복합동사",
        ex: [
          { jp: "<ruby>晩<rt>ばん</rt></ruby>ご<ruby>飯<rt>はん</rt></ruby>を <ruby>食<rt>た</rt></ruby>べおわってから、みんなで ゲームを した。", tts: "晩ご飯を食べおわってから、みんなでゲームをした。", ko: "저녁밥을 다 먹고 나서 모두 함께 게임을 했다." },
          { jp: "この <ruby>本<rt>ほん</rt></ruby>を <ruby>読<rt>よ</rt></ruby>みおわったら、かして ください。", tts: "この本を読みおわったら、かしてください。", ko: "이 책을 다 읽으면 빌려주세요." }
        ]
      },
      {
        id: "031", title: "~がする", mean: "~이 나다, ~이 들다 (감각·느낌)", cat: "일반문형",
        conn: "감각 명사 + がする",
        point: "함께 자주 쓰이는 명사: 味(맛)・音(소리)・匂い(냄새)・気(느낌)・声(목소리)がする",
        ex: [
          { jp: "げんかんで 「ごめんください」という <ruby>声<rt>こえ</rt></ruby>が しました。", tts: "げんかんで「ごめんください」という声がしました。", ko: "현관에서 '실례합니다'라는 소리가 났습니다." },
          { jp: "だいどころから いい においが します。", tts: "だいどころからいいにおいがします。", ko: "부엌에서 좋은 냄새가 납니다." }
        ]
      },
      {
        id: "032", title: "~かた(方)", mean: "~하는 방법", cat: "일반문형",
        conn: "동사 ます형 + かた",
        point: "동사 ます형에 붙어 명사화됨 (読み方・書き方・作り方・使い方)",
        ex: [
          { jp: "この <ruby>字<rt>じ</rt></ruby>の <ruby>読<rt>よ</rt></ruby>み<ruby>方<rt>かた</rt></ruby>を <ruby>教<rt>おし</rt></ruby>えて ください。", tts: "この字の読み方を教えてください。", ko: "이 글자의 읽는 법을 가르쳐 주세요." },
          { jp: "この きかいの <ruby>使<rt>つか</rt></ruby>い<ruby>方<rt>かた</rt></ruby>が わかりません。", tts: "このきかいの使い方がわかりません。", ko: "이 기계의 사용법을 모르겠습니다." }
        ]
      },
      {
        id: "033", title: "~がっている", mean: "~워하고 있다, ~하고 싶어하고 있다", cat: "의지·희망",
        conn: "감정·희망 형용사 어간(ほしい・たい 등) + がっている",
        point: "제3자의 감정이나 희망을 객관적으로 관찰하여 묘사할 때 사용",
        ex: [
          { jp: "<ruby>走<rt>はし</rt></ruby>った あとで、<ruby>子<rt>こ</rt></ruby>どもたちが つめたい <ruby>水<rt>みず</rt></ruby>を ほしがって いる。", tts: "走ったあとで、子どもたちがつめたい水をほしがっている。", ko: "달린 뒤에 아이들이 차가운 물을 마시고 싶어하고 있다." },
          { jp: "いぬが <ruby>外<rt>そと</rt></ruby>へ <ruby>出<rt>で</rt></ruby>たがって います。", tts: "いぬが外へ出たがっています。", ko: "개가 밖으로 나가고 싶어하고 있습니다." }
        ]
      },
      {
        id: "034", title: "~かどうか", mean: "~인지 아닌지, ~할지 어떨지", cat: "일반문형",
        conn: "동사·형용사·명사 보통형 + かどうか",
        point: "의문사가 없는 의문문을 문장 안에 내포할 때 사용 (의문사가 있으면 '의문사+か')",
        ex: [
          { jp: "むすめが この プレゼントを よろこぶかどうか わかりません。", tts: "むすめがこのプレゼントをよろこぶかどうかわかりません。", ko: "딸이 이 선물을 기뻐할지 어떨지 모르겠습니다." },
          { jp: "あしたの <ruby>会議<rt>かいぎ</rt></ruby>に <ruby>出<rt>で</rt></ruby>られるかどうか <ruby>教<rt>おし</rt></ruby>えて ください。", tts: "あしたの会議に出られるかどうか教えてください。", ko: "내일 회의에 참석할 수 있을지 어떨지 알려 주세요." }
        ]
      },
      {
        id: "035", title: "~かもしれない", mean: "~지도 모른다 (불확실한 추측)", cat: "추측·전문",
        conn: "동사·형용사·명사 보통형(な형용사·명사 だ 생략) + かもしれない",
        point: "유사 추측 표현: ~だろう(~일 것이다) / ~でしょう(~일 것입니다)",
        ex: [
          { jp: "<ruby>先生<rt>せんせい</rt></ruby>に いくら たのんでも だめかも しれません。", tts: "先生にいくらたのんでもだめかもしれません。", ko: "선생님에게 아무리 부탁해도 안 될지도 모릅니다." },
          { jp: "ごごから <ruby>雨<rt>あめ</rt></ruby>が <ruby>降<rt>ふ</rt></ruby>るかも しれません。", tts: "ごごから雨が降るかもしれません。", ko: "오후부터 비가 올지도 모릅니다." }
        ]
      },
      {
        id: "036", title: "~くする / ~にする", mean: "~하게 하다 (인위적 변화)", cat: "일반문형",
        conn: "い형용사 어간 + くする / な형용사 어간·명사 + にする",
        point: "형용사의 부사화 활용! 주어가 의도를 가지고 상태를 변화시킬 때 사용",
        ex: [
          { jp: "へやを <ruby>明<rt>あか</rt></ruby>るく しました。", tts: "へやを明るくしました。", ko: "방을 밝게 했습니다." },
          { jp: "<ruby>公園<rt>こうえん</rt></ruby>を きれいに しました。", tts: "公園をきれいにしました。", ko: "공원을 깨끗하게 했습니다." }
        ]
      },
      {
        id: "037", title: "~くなる / ~になる", mean: "~해지다, ~이 되다 (자연적 상태 변화)", cat: "일반문형",
        conn: "い형용사 어간 + くなる / な형용사 어간·명사 + になる",
        point: "상태가 자연스럽게 변한 결과를 나타냄 (~くする/~にする와 짝지어 암기)",
        ex: [
          { jp: "へやが <ruby>明<rt>あか</rt></ruby>るく なりました。", tts: "へやが明るくなりました。", ko: "방이 밝아졌습니다." },
          { jp: "<ruby>公園<rt>こうえん</rt></ruby>が きれいに なりました。", tts: "公園がきれいになりました。", ko: "공원이 깨끗해졌습니다." }
        ]
      },
      {
        id: "038", title: "~こと", mean: "~일, ~것 (문장의 명사화)", cat: "일반문형",
        conn: "동사·형용사 보통형 + こと",
        point: "관련표현: ~の (~こと는 추상적 사실·내용, ~の는 감각·구체적 동작에 주로 사용)",
        ex: [
          { jp: "<ruby>私<rt>わたし</rt></ruby>が ここに いることを だれから <ruby>聞<rt>き</rt></ruby>きましたか。", tts: "私がここにいることをだれから聞きましたか。", ko: "내가 여기에 있는 것을 누구에게 들었습니까?" },
          { jp: "わたしの しゅみは <ruby>音楽<rt>おんがく</rt></ruby>を <ruby>聞<rt>き</rt></ruby>くことです。", tts: "わたしのしゅみは音楽を聞くことです。", ko: "제 취미는 음악을 듣는 것입니다." }
        ]
      },
      {
        id: "039", title: "~ことがある", mean: "~할 때가 있다, ~하는 경우가 있다", cat: "일반문형",
        conn: "동사 사전형 + ことがある",
        point: "주의! 과거형(~たことがある)은 '~한 적이 있다(경험)', 사전형(~ることがある)은 '가끔 ~할 때가 있다'!",
        ex: [
          { jp: "ときどき <ruby>学校<rt>がっこう</rt></ruby>を <ruby>休<rt>やす</rt></ruby>むことが あります。", tts: "ときどき学校を休むことがあります。", ko: "가끔 학교를 쉴 때가 있습니다." },
          { jp: "たまには <ruby>外<rt>そと</rt></ruby>で ご<ruby>飯<rt>はん</rt></ruby>を <ruby>食<rt>た</rt></ruby>べることが あります。", tts: "たまには外でご飯を食べることがあります。", ko: "가끔은 밖에서 밥을 먹을 때가 있습니다." }
        ]
      },
      {
        id: "040", title: "~ことができる", mean: "~할 수(가) 있다 (가능)", cat: "사역·수동",
        conn: "동사 사전형 + ことができる / 명사 + ができる",
        point: "가능동사(~(ら)れる, 113번)와 바꿔 쓸 수 있는 기본 가능 표현",
        ex: [
          { jp: "あなたは <ruby>日本語<rt>にほんご</rt></ruby>で <ruby>手紙<rt>てがみ</rt></ruby>を <ruby>書<rt>か</rt></ruby>くことが できますか。", tts: "あなたは日本語で手紙を書くことができますか。", ko: "당신은 일본어로 편지를 쓸 수 있습니까?" },
          { jp: "ここでは カードで はらうことが できます。", tts: "ここではカードではらうことができます。", ko: "여기서는 카드로 계산할 수 있습니다." }
        ]
      },
      {
        id: "041", title: "~ことにする", mean: "~하기로 하다 (자발적 결정)", cat: "의지·희망",
        conn: "동사 사전형 / ない형 + ことにする",
        point: "화자 자신의 의지로 내린 결정을 나타냄",
        ex: [
          { jp: "わたしは <ruby>毎日<rt>まいにち</rt></ruby> ジョギングを することに しました。", tts: "わたしは毎日ジョギングをすることにしました。", ko: "나는 매일 조깅을 하기로 했습니다." },
          { jp: "けんこうの ために、あまい ものは <ruby>食<rt>た</rt></ruby>べないことに しました。", tts: "けんこうのために、あまいものは食べないことにしました。", ko: "건강을 위해 단것은 먹지 않기로 했습니다." }
        ]
      },
      {
        id: "042", title: "~ことになる / ~ことになっている", mean: "~하게 되다 / ~하기로 되어 있다", cat: "의지·희망",
        conn: "동사 사전형 + ことになる / ことになっている",
        point: "외부 결정·확정(ことになる) vs 규칙·예정·기정사실(ことになっている)",
        ex: [
          { jp: "わたしは らいねん、<ruby>日本<rt>にほん</rt></ruby>へ <ruby>行<rt>い</rt></ruby>くことに なりました。", tts: "わたしはらいねん、日本へ行くことになりました。", ko: "나는 내년에 일본에 가게 되었습니다." },
          { jp: "この <ruby>学校<rt>がっこう</rt></ruby>では 2か<ruby>月<rt>げつ</rt></ruby>に いちど <ruby>試験<rt>しけん</rt></ruby>を することに なって います。", tts: "この学校では2か月にいちど試験をすることになっています。", ko: "이 학교에서는 두 달에 한 번 시험을 치게 되어 있습니다." }
        ]
      },
      {
        id: "043", title: "~さ", mean: "~함, ~기 (형용사의 명사화)", cat: "일반문형",
        conn: "い형용사·な형용사 어간 + さ",
        point: "정도나 수치를 나타내는 명사: 軽い➔かるさ, 広い➔ひろさ, 高い➔たかさ, 長い➔ながさ, 重い➔おもさ, にぎやかだ➔にぎやかさ",
        ex: [
          { jp: "この いけは、ふかさが 2メートルぐらい ありそうです。", tts: "このいけは、ふかさが2メートルぐらいありそうです。", ko: "이 연못은 깊이가 2미터 정도 되는 것 같습니다." },
          { jp: "この <ruby>山<rt>やま</rt></ruby>の <ruby>高<rt>たか</rt></ruby>さは どのくらいですか。", tts: "この山の高さはどのくらいですか。", ko: "이 산의 높이는 어느 정도입니까?" }
        ]
      },
      {
        id: "044", title: "~し", mean: "~하고, ~하니까 (열거·이유)", cat: "일반문형",
        conn: "동사·형용사·명사 보통형(종지형) + し",
        point: "여러 가지 특징을 나열(열거)하거나 여러 이유를 들어 결론을 말할 때 사용 (조사 も와 자주 호응)",
        ex: [
          { jp: "ばらの <ruby>花<rt>はな</rt></ruby>は <ruby>色<rt>いろ</rt></ruby>も きれいだし、においも いいです。", tts: "ばらの花は色もきれいだし、においもいいです。", ko: "장미꽃은 색깔도 예쁘고 향기도 좋습니다. (열거)" },
          { jp: "あたまが いたいし、ねつが あるから、きょうは <ruby>休<rt>やす</rt></ruby>みます。", tts: "あたまがいたいし、ねつがあるから、きょうは休みます。", ko: "머리가 아프고 열이 있으니 오늘은 쉬겠습니다. (이유)" }
        ]
      },
      {
        id: "045", title: "~すぎる", mean: "너무 ~하다, 지나치게 ~하다", cat: "일반문형",
        conn: "동사 ます형 / 형용사 어간 + すぎる",
        point: "복합동사로 2그룹(루동사) 활용을 함 (大きすぎて・食べすぎた)",
        ex: [
          { jp: "この かばんは <ruby>大<rt>おお</rt></ruby>きすぎて ふべんです。", tts: "このかばんは大きすぎてふべんです。", ko: "이 가방은 너무 커서 불편합니다." },
          { jp: "きのうは おさけを <ruby>飲<rt>の</rt></ruby>みすぎました。", tts: "きのうはおさけを飲みすぎました。", ko: "어제는 술을 너무 많이 마셨습니다." }
        ]
      },
      {
        id: "046", title: "~ずに・~ないで", mean: "~하지 않고, ~하지 말고", cat: "일반문형",
        conn: "동사 ない형 어간 + ずに・ないで (※ する ➔ せずに)",
        point: "~ないで보다 ~ずに가 조금 더 문어체적인 표현",
        ex: [
          { jp: "<ruby>兄<rt>あに</rt></ruby>は かばんも <ruby>持<rt>も</rt></ruby>たずに <ruby>家<rt>いえ</rt></ruby>を <ruby>出<rt>で</rt></ruby>ました。", tts: "兄はかばんも持たずに家を出ました。", ko: "형은 가방도 안 들고 집을 나갔습니다." },
          { jp: "わたしは けさ ご<ruby>飯<rt>はん</rt></ruby>を <ruby>食<rt>た</rt></ruby>べないで <ruby>来<rt>き</rt></ruby>ました。", tts: "わたしはけさご飯を食べないで来ました。", ko: "나는 오늘 아침에 밥을 먹지 않고 왔습니다." }
        ]
      },
      {
        id: "047", title: "~(さ)せてください", mean: "~하게 해 주세요 (허락 요청)", cat: "사역·수동",
        conn: "동사 사역형(ない형 어간 + せて/させて) + ください",
        point: "내가 어떤 행동을 하는 것에 대해 상대방의 허락·양해를 구할 때 사용",
        ex: [
          { jp: "ねつが あるので、きょうは <ruby>体育<rt>たいいく</rt></ruby>の じゅぎょうを <ruby>休<rt>やす</rt></ruby>ませて ください。", tts: "ねつがあるので、きょうは体育のじゅぎょうを休ませてください。", ko: "열이 있으니 오늘은 체육 수업을 쉬게 해 주세요." },
          { jp: "ちょっと <ruby>考<rt>かんが</rt></ruby>えさせて ください。", tts: "ちょっと考えさせてください。", ko: "잠시 생각 좀 하게 해 주세요." }
        ]
      },
      {
        id: "048", title: "~(さ)せられる", mean: "억지로 ~하다, ~하게 되다 (사역수동)", cat: "사역·수동",
        conn: "동사 ない형 어간 + (さ)せられる (1그룹 축약: ~される)",
        point: "원하지 않는데 남에 의해 어쩔 수 없이 어떤 행동을 하게 될 때 사용",
        ex: [
          { jp: "かのじょは <ruby>子<rt>こ</rt></ruby>どもの ころ、お<ruby>母<rt>かあ</rt></ruby>さんに いろいろな <ruby>野菜<rt>やさい</rt></ruby>を <ruby>食<rt>た</rt></ruby>べさせられました。", tts: "かのじょは子どものころ、お母さんにいろいろな野菜を食べさせられました。", ko: "그녀는 어렸을 적에 어머니에게 억지로 여러 채소를 먹어야 했습니다." },
          { jp: "ぶちょうに おさけを <ruby>飲<rt>の</rt></ruby>ませられました。", tts: "ぶちょうにおさけを飲ませられました。", ko: "부장님에게 억지로 술을 마시게 되었습니다." }
        ]
      },
      {
        id: "049", title: "~(さ)せる", mean: "~하게 하다, 시키다 (사역)", cat: "사역·수동",
        conn: "1그룹: ない형 어간+せる / 2그룹: ない형 어간+させる / する➔させる, 来る➔こさせる",
        point: "강제(시키다), 허용(하게 두다), 감정유발(기쁘게/안심하게 하다), 책임 등을 나타냄",
        ex: [
          { jp: "かれは <ruby>子<rt>こ</rt></ruby>どもに くつを はかせた。", tts: "かれは子どもにくつをはかせた。", ko: "그는 아이에게 신발을 신게 했다." },
          { jp: "むすめを アメリカへ りゅうがくさせました。", tts: "むすめをアメリカへりゅうがくさせました。", ko: "딸을 미국으로 유학 보냈습니다(유학하게 했습니다)." }
        ]
      },
      {
        id: "050", title: "~そうだ", mean: "~한 듯하다(양태), ~라고 한다(전문)", cat: "추측·전문",
        conn: "[양태] 동사 ます형·형용사 어간 + そうだ (예외: なさそうだ・よさそうだ) / [전문] 보통형 + そうだ",
        point: "접속 형태에 따라 뜻이 완전히 달라지므로 시험 출제율 1순위!",
        ex: [
          { jp: "<ruby>鈴木<rt>すずき</rt></ruby>さんは ビールを おいしそうに <ruby>飲<rt>の</rt></ruby>んで いる。", tts: "鈴木さんはビールをおいしそうに飲んでいる。", ko: "스즈키 씨는 맥주를 맛있어 보이게 마시고 있습니다. (양태)" },
          { jp: "ニュースに よると、きのう <ruby>神戸<rt>こうべ</rt></ruby>で じしんが あったそうです。", tts: "ニュースによると、きのう神戸でじしんがあったそうです。", ko: "뉴스에 따르면 어제 고베에서 지진이 있었다고 합니다. (전문)" }
        ]
      },
      // ==========================================
      // [2단계 추가 범위] 051 ~ 114번
      // ==========================================
      {
        id: "051", title: "~たい / ~たがる", mean: "~하고 싶다 (1·2인칭) / ~하고 싶어하다 (3인칭)", cat: "의지·희망",
        conn: "동사 ます형 + たい / たがる",
        point: "~たい는 い형용사 활용(내가 하고 싶다), ~たがる는 1그룹 동사 활용(제3자가 하고 싶어하다)",
        ex: [
          { jp: "わたしは <ruby>先生<rt>せんせい</rt></ruby>に なりたいです。", tts: "わたしは先生になりたいです。", ko: "나는 선생님이 되고 싶습니다." },
          { jp: "あんな あつい <ruby>所<rt>ところ</rt></ruby>へは だれも <ruby>行<rt>い</rt></ruby>きたがらないだろう。", tts: "あんなあつい所へはだれも行きたがらないだろう。", ko: "저런 더운 곳에는 아무도 가고 싶어하지 않을 것이다." }
        ]
      },
      {
        id: "052", title: "~たことがある", mean: "~한 적이 있다 (과거 경험)", cat: "일반문형",
        conn: "동사 た형(과거형) + ことがある",
        point: "경험의 유무를 나타냄 (사전형 + ことがある는 '가끔 ~할 때가 있다'이므로 혼동 주의!)",
        ex: [
          { jp: "わたしは ひこうきに <ruby>乗<rt>の</rt></ruby>った ことが あります。", tts: "わたしはひこうきに乗ったことがあります。", ko: "나는 비행기를 탄 적이 있습니다." },
          { jp: "ふじ<ruby>山<rt>さん</rt></ruby>に <ruby>登<rt>のぼ</rt></ruby>った ことが ありますか。", tts: "ふじ山に登ったことがありますか。", ko: "후지산에 올라간 적이 있습니까?" }
        ]
      },
      {
        id: "053", title: "~だす", mean: "(갑자기) ~하기 시작하다", cat: "일반문형",
        conn: "동사 ます형 + だす",
        point: "유사표현: ~はじめる (~だす는 갑작스러운 시작에, ~はじめる는 일반적인 시작에 주로 사용)",
        ex: [
          { jp: "<ruby>寝<rt>ね</rt></ruby>て いた <ruby>赤<rt>あか</rt></ruby>ちゃんが <ruby>急<rt>きゅう</rt></ruby>に <ruby>泣<rt>な</rt></ruby>きだした。", tts: "寝ていた赤ちゃんが急に泣きだした。", ko: "자고 있던 갓난아이가 갑자기 울기 시작했다." },
          { jp: "とつぜん <ruby>雨<rt>あめ</rt></ruby>が <ruby>降<rt>ふ</rt></ruby>りだしました。", tts: "とつぜん雨が降りだしました。", ko: "갑자기 비가 내리기 시작했습니다." }
        ]
      },
      {
        id: "054", title: "~たところだ", mean: "막 ~한 참이다 (완료 직후)", cat: "일반문형",
        conn: "동사 た형(과거형) + ところだ",
        point: "함께 자주 쓰는 부사: たった今(방금 막)・ちょうど(마침)・今(지금)",
        ex: [
          { jp: "かいぎは <ruby>今<rt>いま</rt></ruby> <ruby>終<rt>お</rt></ruby>わった ところです。", tts: "かいぎは今終わったところです。", ko: "회의는 지금 막 끝난 참입니다." },
          { jp: "たった<ruby>今<rt>いま</rt></ruby> <ruby>駅<rt>えき</rt></ruby>に <ruby>着<rt>つ</rt></ruby>いた ところです。", tts: "たった今駅に着いたところです。", ko: "방금 막 역에 도착한 참입니다." }
        ]
      },
      {
        id: "055", title: "~たまま", mean: "~한 채로, ~한 상태로", cat: "일반문형",
        conn: "동사 た형 / 명사の + まま",
        point: "이전 동작의 상태가 그대로 유지된 채 다음 동작이 일어날 때 사용",
        ex: [
          { jp: "かれは めがねを かけた まま <ruby>寝<rt>ね</rt></ruby>て いる。", tts: "かれはめがねをかけたまま寝ている。", ko: "그는 안경을 쓴 채로 자고 있다." },
          { jp: "テレビを つけた まま <ruby>出<rt>で</rt></ruby>かけて しまった。", tts: "テレビをつけたまま出かけてしまった。", ko: "TV를 켜 놓은 채로 외출해 버렸다." }
        ]
      },
      {
        id: "056", title: "~ため(に)", mean: "~하기 위해서(목적), ~때문에(이유)", cat: "일반문형",
        conn: "[목적] 동사 사전형·명사の + ために / [이유] 동사 과거형·형용사·명사の + ため(に)",
        point: "동사 사전형이 오면 '목적(~하기 위해)', 과거형이나 상태 표현이 오면 '원인·이유(~때문에)'!",
        ex: [
          { jp: "<ruby>大学<rt>だいがく</rt></ruby>に <ruby>入<rt>はい</rt></ruby>る ために いっしょうけんめい <ruby>勉強<rt>べんきょう</rt></ruby>して います。", tts: "大学に入るためにいっしょうけんめい勉強しています。", ko: "대학에 들어가기 위해서 열심히 공부하고 있습니다. (목적)" },
          { jp: "<ruby>病気<rt>びょうき</rt></ruby>の ため、たばこを やめます。", tts: "病気のため、たばこをやめます。", ko: "병 때문에 담배를 끊습니다. (이유)" }
        ]
      },
      {
        id: "057", title: "~たら", mean: "~하면, ~하니까, ~하였더니", cat: "가정·조건",
        conn: "동사·형용사·명사 과거형(た/だった) + ら",
        point: "가정 조건(만약 ~하면),確定 조건(~하고 나면), 우연한 발견(~했더니)에 두루 쓰임 (부사 もし와 호응)",
        ex: [
          { jp: "<ruby>山田<rt>やまだ</rt></ruby>さんに <ruby>道<rt>みち</rt></ruby>を <ruby>聞<rt>き</rt></ruby>いたら、しんせつに ちずを かいて くれました。", tts: "山田さんに道を聞いたら、しんせつにちずをかいてくれました。", ko: "야마다 씨에게 길을 물었더니 친절하게 지도를 그려 주었습니다." },
          { jp: "<ruby>東京<rt>とうきょう</rt></ruby>に <ruby>着<rt>つ</rt></ruby>いたら、<ruby>電話<rt>でんわ</rt></ruby>を して ください。", tts: "東京に着いたら、電話をしてください。", ko: "도쿄에 도착하면 전화를 해 주세요." }
        ]
      },
      {
        id: "058", title: "~だろう", mean: "~일 것이다 (추측)", cat: "추측·전문",
        conn: "동사·い형용사 보통형 / な형용사 어간·명사 + だろう",
        point: "부사 たぶん(아마)과 자주 호응하며, 정중형은 ~でしょう (068번)",
        ex: [
          { jp: "あしたは たぶん いい <ruby>天気<rt>てんき</rt></ruby>だろう。", tts: "あしたはたぶんいい天気だろう。", ko: "내일은 아마 날씨가 좋을 것이다." },
          { jp: "この もんだいは <ruby>小学生<rt>しょうがくせい</rt></ruby>には むずかしいだろう。", tts: "このもんだいは小学生にはむずかしいだろう。", ko: "이 문제는 초등학생에게는 어려울 것이다." }
        ]
      },
      {
        id: "059", title: "~ちゃ / ~なくちゃ・~なきゃ", mean: "~해서는 / ~하지 않으면", cat: "의무·허가",
        conn: "동사 て형 ➔ ちゃ(じゃ) / 동사 ない형 ➔ なくちゃ・なきゃ",
        point: "회화체 축약형 필수 암기! (~ては ➔ ~ちゃ / ~なくては ➔ ~なくちゃ / ~なければ ➔ ~なきゃ)",
        ex: [
          { jp: "ここで たばこを すっちゃ だめだよ。", tts: "ここでたばこをすっちゃだめだよ。", ko: "여기서 담배를 피워서는 안 돼." },
          { jp: "できるか どうかは やって みなきゃ わからない。", tts: "できるかどうかはやってみなきゃわからない。", ko: "할 수 있을지 없을지는 해 보지 않으면 모른다." }
        ]
      },
      {
        id: "060", title: "~ちゅう・じゅう(中)", mean: "~하는 중(ちゅう), ~내내·전역(じゅう)", cat: "일반문형",
        conn: "명사 + 中(ちゅう / じゅう)",
        point: "[ちゅう] 旅行中・会議中・工事中・授業中・今週中(이번 주 안) / [じゅう] 一日中・一晩中・一年中・世界中",
        ex: [
          { jp: "イさんは いま べんきょう<ruby>中<rt>ちゅう</rt></ruby>です。", tts: "イさんはいまべんきょう中です。", ko: "이 씨는 지금 공부 중입니다." },
          { jp: "きのうは <ruby>一日中<rt>いちにちじゅう</rt></ruby> <ruby>雨<rt>あめ</rt></ruby>が <ruby>降<rt>ふ</rt></ruby>って いました。", tts: "きのうは一日中雨が降っていました。", ko: "어제는 하루 종일 비가 내리고 있었습니다." }
        ]
      },
      {
        id: "061", title: "~つづける", mean: "계속 ~하다", cat: "일반문형",
        conn: "동사 ます형 + つづける",
        point: "어떤 동작이나 상태를 멈추지 않고 지속함을 나타내는 복합동사",
        ex: [
          { jp: "2<ruby>時間<rt>じかん</rt></ruby>も <ruby>歩<rt>ある</rt></ruby>きつづけたので、たいへん つかれました。", tts: "2時間も歩きつづけたので、たいへんつかれました。", ko: "두 시간이나 계속 걸었기 때문에 몹시 지쳤습니다." },
          { jp: "かれは 3<ruby>時間<rt>じかん</rt></ruby> うたいつづけた。", tts: "かれは3時間うたいつづけた。", ko: "그는 3시간 동안 계속 노래를 불렀다." }
        ]
      },
      {
        id: "062", title: "~つもりだ", mean: "~할 생각이다, ~할 작정이다", cat: "의지·희망",
        conn: "동사 사전형 / ない형 + つもりだ",
        point: "화자의 강한 계획이나 의향을 나타냄 (~ないつもりだ: ~하지 않을 생각이다)",
        ex: [
          { jp: "あしたは どこへも <ruby>行<rt>い</rt></ruby>かない つもりです。", tts: "あしたはどこへも行かないつもりです。", ko: "내일은 아무 데도 가지 않을 생각입니다." },
          { jp: "<ruby>大学<rt>だいがく</rt></ruby>を <ruby>卒業<rt>そつぎょう</rt></ruby>したら、くにへ <ruby>帰<rt>かえ</rt></ruby>る つもりです。", tts: "大学を卒業したら、くにへ帰るつもりです。", ko: "대학을 졸업하면 고국으로 돌아갈 생각입니다." }
        ]
      },
      {
        id: "063", title: "~て・で", mean: "~하고, ~해서 (순차·이유·병렬)", cat: "일반문형",
        conn: "동사·い형용사 て형(大きくて) / な형용사·명사 + で(好きで・雨で)",
        point: "동작의 시간적 순서(~하고 나서) 또는 가벼운 원인·이유(~해서)를 연결",
        ex: [
          { jp: "あさ <ruby>起<rt>お</rt></ruby>きて しんぶんを <ruby>読<rt>よ</rt></ruby>みます。", tts: "あさ起きてしんぶんを読みます。", ko: "아침에 일어나서 신문을 읽습니다." },
          { jp: "きのうは <ruby>雨<rt>あめ</rt></ruby>で、どこへも <ruby>行<rt>い</rt></ruby>けませんでした。", tts: "きのうは雨で、どこへも行けませんでした。", ko: "어제는 비가 와서 아무 데도 갈 수 없었습니다." }
        ]
      },
      {
        id: "064", title: "~てあげる", mean: "~해 주다 (남에게 베풂)", cat: "수수표현",
        conn: "동사 て형 + あげる (겸양어: ~てさしあげる)",
        point: "나(또는 제3자)가 다른 사람에게 무언가를 해 줄 때 사용 (비교: 남이 나에게 해 주는 건 ~てくれる)",
        ex: [
          { jp: "わたしは <ruby>友<rt>とも</rt></ruby>だちに <ruby>写真<rt>しゃしん</rt></ruby>を おくって あげた。", tts: "わたしは友だちに写真をおくってあげた。", ko: "나는 친구에게 사진을 보내 주었다." },
          { jp: "お<ruby>年寄<rt>としよ</rt></ruby>りに <ruby>席<rt>せき</rt></ruby>を ゆずって さしあげました。", tts: "お年寄りに席をゆずってさしあげました。", ko: "어르신께 자리를 양보해 드렸습니다." }
        ]
      },
      {
        id: "065", title: "~ていく", mean: "~해 가다, (앞으로) ~해 나가다", cat: "일반문형",
        conn: "동사 て형 + いく",
        point: "화자로부터 멀어지는 이동(~해서 가다) 또는 현재부터 미래로 이어지는 지속(~해 나가다)",
        ex: [
          { jp: "<ruby>山田<rt>やまだ</rt></ruby>さんは こうばんまで <ruby>走<rt>はし</rt></ruby>って いきました。", tts: "山田さんはこうばんまで走っていきました。", ko: "야마다 씨는 파출소까지 달려갔습니다." },
          { jp: "けっこんしてからも <ruby>仕事<rt>しごと</rt></ruby>は つづけて いく つもりです。", tts: "けっこんしてからも仕事はつづけていくつもりです。", ko: "결혼하고 나서도 일은 계속해 나갈 생각입니다." }
        ]
      },
      {
        id: "066", title: "~ていただく", mean: "~해 받다 (겸양 수수표현)", cat: "수수표현",
        conn: "동사 て형 + いただく",
        point: "~てもらう의 겸양어로, 윗사람에게 은혜나 행동을 받았을 때 사용",
        ex: [
          { jp: "わたしは <ruby>高校<rt>こうこう</rt></ruby>の とき、<ruby>田中先生<rt>たなかせんせい</rt></ruby>に <ruby>英語<rt>えいご</rt></ruby>を <ruby>教<rt>おし</rt></ruby>えて いただきました。", tts: "わたしは高校のとき、田中先生に英語を教えていただきました。", ko: "나는 고등학교 때 다나카 선생님께 영어를 배웠습니다(가르침을 받았습니다)." },
          { jp: "ぶちょうに まちがいを なおして いただきました。", tts: "ぶちょうにまちがいをなおしていただきました。", ko: "부장님께 틀린 부분을 수정받았습니다." }
        ]
      },
      {
        id: "067", title: "~ている / ~てある", mean: "~하고 있다, ~되어 있다 / ~해져 있다", cat: "일반문형",
        conn: "자동사 て형 + いる / 타동사 て형 + ある",
        point: "자동사+ている(자연적 결과 상태) vs 타동사+てある(누군가 의도적으로 해 놓은 상태, 조사 を➔が 변화 주의!)",
        ex: [
          { jp: "まどが しまって います。", tts: "まどがしまっています。", ko: "창문이 닫혀 있습니다. (자동사 + ている)" },
          { jp: "テーブルの うえに スプーンと はしが ならべて あります。", tts: "テーブルのうえにスプーンとはしがならべてあります。", ko: "탁자 위에 숟가락과 젓가락이 가지런히 놓여져 있습니다. (타동사 + てある)" }
        ]
      },
      {
        id: "068", title: "~ているところだ", mean: "지금 ~하고 있는 중이다", cat: "일반문형",
        conn: "동사 て형 + いるところだ",
        point: "현재 동작이 한창 진행 중임을 강조할 때 사용 (부사 今과 자주 호응)",
        ex: [
          { jp: "わたしは <ruby>今<rt>いま</rt></ruby> れきしの <ruby>本<rt>ほん</rt></ruby>を <ruby>読<rt>よ</rt></ruby>んで いる ところです。", tts: "わたしは今れきしの本を読んでいるところです。", ko: "나는 지금 역사책을 읽고 있는 중입니다." },
          { jp: "<ruby>今<rt>いま</rt></ruby>、ばんご<ruby>飯<rt>はん</rt></ruby>を <ruby>作<rt>つく</rt></ruby>って いる ところです。", tts: "今、ばんご飯を作っているところです。", ko: "지금 저녁밥을 만들고 있는 중입니다." }
        ]
      },
      {
        id: "069", title: "~ておく", mean: "(미리) ~해 놓다, ~해 두다", cat: "일반문형",
        conn: "동사 て형 + おく (회화 축약형: ~とく / ~どく)",
        point: "다음 일의 준비를 위해 미리 어떤 동작을 해 두거나 그 상태를 유지해 둘 때 사용",
        ex: [
          { jp: "この ことばは だいじですから、よく <ruby>覚<rt>おぼ</rt></ruby>えて おいて ください。", tts: "このことばはだいじですから、よく覚えておいてください。", ko: "이 말은 중요하니까 잘 기억해 두세요." },
          { jp: "テープは あとで <ruby>使<rt>つか</rt></ruby>いますから、ここに ならべて おいて ください。", tts: "テープはあとで使いますから、ここにならべておいてください。", ko: "테이프는 나중에 사용할 테니까 여기에 진열해 놓아 주세요." }
        ]
      },
      {
        id: "070", title: "~てから / ~たあとで / ~まえ(前)に", mean: "~하고 나서 / ~한 후에 / ~하기 전에", cat: "일반문형",
        conn: "て형+から / 과거형(た형)·명사の+あとで / 사전형·명사の·시간명사+まえに",
        point: "명사 접속 시: 食事の前に(식사 전에), 食事の後で(식사 후에), 3年前に(3년 전에)",
        ex: [
          { jp: "ご<ruby>飯<rt>はん</rt></ruby>を <ruby>食<rt>た</rt></ruby>べてから（<ruby>食<rt>た</rt></ruby>べた あとで） おふろに <ruby>入<rt>はい</rt></ruby>ります。", tts: "ご飯を食べてからおふろに入ります。", ko: "밥을 먹고 나서(먹은 후에) 목욕을 합니다." },
          { jp: "おふろに <ruby>入<rt>はい</rt></ruby>る <ruby>前<rt>まえ</rt></ruby>に ご<ruby>飯<rt>はん</rt></ruby>を <ruby>食<rt>た</rt></ruby>べます。", tts: "おふろに入る前にご飯を食べます。", ko: "목욕을 하기 전에 밥을 먹습니다." }
        ]
      },
      {
        id: "071", title: "~てくださる / ~てくださいませんか", mean: "~해 주시다 / ~해 주시지 않겠습니까?", cat: "수수표현",
        conn: "동사 て형 + くださる / くださいませんか",
        point: "~てくれる의 존경어! (くださる의 ます형은 くださいます로 변하는 특수 활용 주의)",
        ex: [
          { jp: "きのうは <ruby>山田<rt>やまだ</rt></ruby>さんが <ruby>東京<rt>とうきょう</rt></ruby>を あんないして くださいました。", tts: "きのうは山田さんが東京をあんないしてくださいました。", ko: "어제는 야마다 씨가 도쿄를 안내해 주셨습니다." },
          { jp: "<ruby>美術館<rt>びじゅつかん</rt></ruby>へ <ruby>行<rt>い</rt></ruby>く <ruby>道<rt>みち</rt></ruby>を <ruby>教<rt>おし</rt></ruby>えて くださいませんか。", tts: "美術館へ行く道を教えてくださいませんか。", ko: "미술관에 가는 길을 가르쳐 주시지 않겠습니까?" }
        ]
      },
      {
        id: "072", title: "~てくる", mean: "~하고 오다, (점점) ~해지다", cat: "일반문형",
        conn: "동사 て형 + くる",
        point: "잠시 어디에 가서 동작을 하고 돌아옴(~하고 오다) 또는 과거부터 현재까지의 변화(~해지다)",
        ex: [
          { jp: "ちょっと <ruby>手紙<rt>てがみ</rt></ruby>を <ruby>出<rt>だ</rt></ruby>して きます。", tts: "ちょっと手紙を出してきます。", ko: "잠시 편지를 부치고 오겠습니다." },
          { jp: "あたらしい <ruby>家<rt>いえ</rt></ruby>に だんだん なれて きました。", tts: "あたらしい家にだんだんなれてきました。", ko: "새 집에 점점 익숙해졌습니다." }
        ]
      },
      {
        id: "073", title: "~てくれる", mean: "(나에게) ~해 주다", cat: "수수표현",
        conn: "동사 て형 + くれる",
        point: "제3자가 나(또는 내 가족/우리 측)에게 무언가를 해 줄 때 사용 (윗사람이면 ~てくださる)",
        ex: [
          { jp: "わたしが <ruby>東京<rt>とうきょう</rt></ruby>に <ruby>行<rt>い</rt></ruby>った とき、<ruby>山田<rt>やまだ</rt></ruby>さんが あんないして くれた。", tts: "わたしが東京に行ったとき、山田さんがあんないしてくれた。", ko: "내가 도쿄에 갔을 때, 야마다 씨가 안내해 주었다." },
          { jp: "<ruby>母<rt>はは</rt></ruby>が セーターを <ruby>買<rt>か</rt></ruby>って くれました。", tts: "母がセーターを買ってくれました。", ko: "어머니가 스웨터를 사 주셨습니다." }
        ]
      },
      {
        id: "074", title: "~てしまう", mean: "~해 버리다, ~하고 말다 (완료·후회)", cat: "일반문형",
        conn: "동사 て형 + しまう",
        point: "동작의 완전한 완료 또는 실수·아쉬움·후회를 나타냄 (회화 축약형: ~てしまう➔~ちゃう / ~でしまう➔~じゃう)",
        ex: [
          { jp: "ゆうべ おそく ねたから、けさは あさねぼうを して しまいました。", tts: "ゆうべおそくねたから、けさはあさねぼうをしてしまいました。", ko: "어젯밤 늦게 잤기 때문에 오늘 아침에는 늦잠을 자고 말았습니다." },
          { jp: "パスポートを なくしちゃった。", tts: "パスポートをなくしちゃった。", ko: "여권을 잃어버리고 말았다." }
        ]
      },
      {
        id: "075", title: "~てはいけない", mean: "~해서는 안 된다 (금지)", cat: "의무·허가",
        conn: "동사 て형 + はいけない (정중형: ~てはいけません)",
        point: "규칙이나 도리상 금지할 때 사용 (비교: ~なくてはいけない는 '~하지 않으면 안 된다')",
        ex: [
          { jp: "ここは きけんだから、ここで およいでは いけません。", tts: "ここはきけんだから、ここでおよいではいけません。", ko: "이곳은 위험하니까 여기에서 수영하면 안 됩니다." },
          { jp: "としょかんの <ruby>中<rt>なか</rt></ruby>で たべものを <ruby>食<rt>た</rt></ruby>べては いけません。", tts: "としょかんの中でたべものを食べてはいけません。", ko: "도서관 안에서 음식을 먹어서는 안 됩니다." }
        ]
      },
      {
        id: "076", title: "~てみる", mean: "(시험 삼아) ~해 보다", cat: "일반문형",
        conn: "동사 て형 + みる",
        point: "어떤 동작을 가볍게 시도해 볼 때 사용",
        ex: [
          { jp: "ほかの <ruby>人<rt>ひと</rt></ruby>の いけんを <ruby>聞<rt>き</rt></ruby>いて みましょう。", tts: "ほかの人のいけんを聞いてみましょう。", ko: "다른 사람의 의견을 들어 봅시다." },
          { jp: "この くつを はいて みても いいですか。", tts: "このくつをはいてみてもいいですか。", ko: "이 신발을 신어 봐도 될까요?" }
        ]
      },
      {
        id: "077", title: "~ても", mean: "~하더라도, ~해도 (양보 가정)", cat: "가정·조건",
        conn: "동사·い형용사 て형 + も / な형용사·명사 + でも",
        point: "전제 조건에 구애받지 않음을 나타냄 (부사 いくら・どんなに・たとえ와 자주 호응)",
        ex: [
          { jp: "<ruby>何度<rt>なんど</rt></ruby> しっぱいしても、やめずに がんばります。", tts: "何度しっぱいしても、やめずにがんばります。", ko: "몇 번 실패하더라도 그만두지 않고 열심히 하겠습니다." },
          { jp: "<ruby>雨<rt>あめ</rt></ruby>が <ruby>降<rt>ふ</rt></ruby>っても、しあいは おこないます。", tts: "雨が降っても、しあいはおこないます。", ko: "비가 오더라도 경기는 진행합니다." }
        ]
      },
      {
        id: "078", title: "~てもいい", mean: "~해도 좋다, ~해도 된다 (허가)", cat: "의무·허가",
        conn: "동사·형용사 て형 + もいい (명사·な형용사 + でもいい)",
        point: "유사표현: ~てもかまわない (079번) / 반대개념(불필요): ~なくてもいい (089번)",
        ex: [
          { jp: "<ruby>試験<rt>しけん</rt></ruby>が <ruby>終<rt>お</rt></ruby>わった <ruby>人<rt>ひと</rt></ruby>は <ruby>帰<rt>かえ</rt></ruby>っても いいです。", tts: "試験が終わった人は帰ってもいいです。", ko: "시험이 끝난 사람은 돌아가도 됩니다." },
          { jp: "ここに <ruby>すわっても</ruby> いいですか。", tts: "ここにすわってもいいですか。", ko: "여기에 앉아도 좋습니까?" }
        ]
      },
      {
        id: "079", title: "~てもかまわない", mean: "~해도 상관없다(괜찮다)", cat: "의무·허가",
        conn: "동사·형용사 て형 + もかまわない (명사·な형용사 + でもかまわない)",
        point: "~てもいい와 같은 허가·양보 표현으로 좀 더 격식 있는 어감 (관련표현: ~なくてもかまわない)",
        ex: [
          { jp: "ここでは なにを <ruby>話<rt>はな</rt></ruby>しても かまいません。", tts: "ここではなにを話してもかまいません。", ko: "여기서는 무슨 얘기를 해도 상관없습니다." },
          { jp: "すこし おくれても かまいませんよ。", tts: "すこしおくれてもかまいませんよ。", ko: "조금 늦어도 괜찮아요." }
        ]
      },
      {
        id: "080", title: "~てもらう", mean: "~해 받다 (남에게 부탁해 혜택 받음)", cat: "수수표현",
        conn: "동사 て형 + もらう (겸양어: ~ていただく)",
        point: "주어는 나(화자), 동작을 해 준 상대방에게는 조사 に를 붙임! (父に なおしてもらった = 父が なおしてくれた)",
        ex: [
          { jp: "<ruby>父<rt>ちち</rt></ruby>に おもちゃを なおして もらった。", tts: "父におもちゃをなおしてもらった。", ko: "아버지에게 장난감을 고침 받았다(아버지가 고쳐 주셨다)." },
          { jp: "<ruby>友<rt>とも</rt></ruby>だちに <ruby>本<rt>ほん</rt></ruby>を かして もらいました。", tts: "友だちに本をかしてもらいました。", ko: "친구에게 책을 빌림 받았다(친구가 빌려주었다)." }
        ]
      },
      {
        id: "081", title: "~てやる", mean: "(아랫사람·동식물에게) ~해 주다", cat: "수수표현",
        conn: "동사 て형 + やる",
        point: "손아랫사람(자녀, 동생)이나 동물, 식물에게 무언가를 해 줄 때 사용 (관련표현: ~てあげる)",
        ex: [
          { jp: "<ruby>子<rt>こ</rt></ruby>どもを どうぶつえんへ <ruby>連<rt>つ</rt></ruby>れて いって やった。", tts: "子どもをどうぶつえんへ連れていってやった。", ko: "아이를 동물원에 데려가 주었다." },
          { jp: "まいにち <ruby>花<rt>はな</rt></ruby>に <ruby>水<rt>みず</rt></ruby>を やって います。", tts: "まいにち花に水をやっています。", ko: "매일 꽃에 물을 주고 있습니다." }
        ]
      },
      {
        id: "082", title: "~と", mean: "~하면, ~하니까 (자연적·필연적 조건)", cat: "가정·조건",
        conn: "동사·형용사 사전형(기본형) + と",
        point: "길 안내, 자연 현상, 기계 작동 등 '~하면 반드시 ~한다'는 필연적 결과에 사용 (뒷문장에 의지·명령문 불가!)",
        ex: [
          { jp: "この <ruby>道<rt>みち</rt></ruby>を まっすぐ <ruby>行<rt>い</rt></ruby>くと、<ruby>右<rt>みぎ</rt></ruby>がわに ほんやが あります。", tts: "この道をまっすぐ行くと、右がわにほんやがあります。", ko: "이 길을 곧장 가면 오른쪽에 서점이 있습니다." },
          { jp: "<ruby>春<rt>はる</rt></ruby>に なると、さくらが さきます。", tts: "春になると、さくらがさきます。", ko: "봄이 되면 벚꽃이 핍니다." }
        ]
      },
      {
        id: "083", title: "~と~とどちら", mean: "~와 ~중 어느 쪽 (양자 비교 선택)", cat: "일반문형",
        conn: "명사 + と + 명사 + と どちら",
        point: "두 가지를 비교해 물을 때 쓰며, 대답은 '~のほうが(쪽이) ...'로 함 (110번 연계)",
        ex: [
          { jp: "コーラと ジュースと どちらが <ruby>好<rt>す</rt></ruby>きですか。", tts: "コーラとジュースとどちらが好きですか。", ko: "콜라와 주스 중 어느 쪽을 좋아합니까?" },
          { jp: "ジュースの ほうが <ruby>好<rt>す</rt></ruby>きです。", tts: "ジュースのほうが好きです。", ko: "주스 쪽을 더 좋아합니다." }
        ]
      },
      {
        id: "084", title: "~とき", mean: "~할 때, ~일 때", cat: "일반문형",
        conn: "동사·い형용사 보통형 / な형용사+な / 명사+の + とき",
        point: "사전형+とき(동작 완료 전), 과거형 た+とき(동작 완료 후)의 시제 차이 주의!",
        ex: [
          { jp: "<ruby>父<rt>ちち</rt></ruby>は <ruby>本<rt>ほん</rt></ruby>を <ruby>読<rt>よ</rt></ruby>む とき いつも めがねを かけます。", tts: "父は本を読むときいつもめがねをかけます。", ko: "아빠는 책을 읽을 때 항상 안경을 씁니다." },
          { jp: "<ruby>子<rt>こ</rt></ruby>どもの とき、よく この <ruby>川<rt>かわ</rt></ruby>で およぎました。", tts: "子どものとき、よくこの川でおよぎました。", ko: "어렸을 때 자주 이 강에서 수영했습니다." }
        ]
      },
      {
        id: "085", title: "~ところだ", mean: "막 ~하려는 참이다 (동작 직전)", cat: "일반문형",
        conn: "동사 사전형 + ところだ",
        point: "관련표현 3총사: 사전형+ところだ(하려는 참), ている+ところだ(하는 중), た+ところだ(막 끝난 참)",
        ex: [
          { jp: "<ruby>今<rt>いま</rt></ruby>から、<ruby>図書館<rt>としょかん</rt></ruby>へ <ruby>本<rt>ほん</rt></ruby>を かえしに <ruby>行<rt>い</rt></ruby>く ところです。", tts: "今から、図書館へ本をかえしに行くところです。", ko: "지금부터 도서관에 책을 반납하러 가려는 참입니다." },
          { jp: "これから ひるご<ruby>飯<rt>はん</rt></ruby>を <ruby>食<rt>た</rt></ruby>べる ところです。", tts: "これからひるご飯を食べるところです。", ko: "이제부터 점심밥을 먹으려는 참입니다." }
        ]
      },
      {
        id: "086", title: "~とちゅう(途中)で", mean: "~하는 도중에", cat: "일반문형",
        conn: "동사 사전형 / 명사の + とちゅうで",
        point: "목적지로 이동하거나 어떤 일이 진행되는 도중에 다른 일이 발생했을 때 사용",
        ex: [
          { jp: "<ruby>学校<rt>がっこう</rt></ruby>に <ruby>来<rt>く</rt></ruby>る とちゅうで さいふを <ruby>忘<rt>わす</rt></ruby>れたのに <ruby>気<rt>き</rt></ruby>づいた。", tts: "学校に来るとちゅうでさいふを忘れたのに気づいた。", ko: "학교에 오는 도중에 지갑을 두고 온 것을 깨달았다." },
          { jp: "しごとの とちゅうで ちょっと コーヒーを <ruby>飲<rt>の</rt></ruby>んだ。", tts: "しごとのとちゅうでちょっとコーヒーを飲んだ。", ko: "일하는 도중에 잠깐 커피를 마셨다." }
        ]
      },
      {
        id: "087", title: "どんなに~ても", mean: "아무리 ~해도", cat: "가정·조건",
        conn: "どんなに + 동사·형용사 て형(명사·な형용사 + でも)",
        point: "유사표현: いくら~ても (023번) — 정도가 아무리 심해도 결과가 바뀌지 않음을 강조",
        ex: [
          { jp: "どんなに <ruby>練習<rt>れんしゅう</rt></ruby>しても、ピアノが うまく なりません。", tts: "どんなに練習しても、ピアノがうまくなりません。", ko: "아무리 연습해도 피아노가 능숙해지지 않습니다." },
          { jp: "どんなに いそがしくても、あさご<ruby>飯<rt>はん</rt></ruby>は かならず <ruby>食<rt>た</rt></ruby>べます。", tts: "どんなにいそがしくても、あさご飯はかならず食べます。", ko: "아무리 바빠도 아침밥은 반드시 먹습니다." }
        ]
      },
      {
        id: "088", title: "~なくてはいけない / ~なければならない", mean: "~하지 않으면 안 된다, ~해야 한다 (의무)", cat: "의무·허가",
        conn: "동사 ない형 어간 + なくてはいけない / なければならない",
        point: "유사표현: ~なくてはならない / ~なければいけない (모두 의무·필요를 나타냄)",
        ex: [
          { jp: "こたえは かならず ボールペンで <ruby>書<rt>か</rt></ruby>かなくては いけません。", tts: "こたえはかならずボールペンで書かなくてはいけません。", ko: "답은 반드시 볼펜으로 쓰지 않으면 안 됩니다." },
          { jp: "3<ruby>時<rt>じ</rt></ruby>までに、<ruby>会社<rt>かいしゃ</rt></ruby>に もどらなければ ならない。", tts: "3時までに、会社にもどらなければならない。", ko: "3시까지 회사에 돌아가지 않으면 안 된다." }
        ]
      },
      {
        id: "089", title: "~なくてもいい", mean: "~하지 않아도 좋다(된다) (불필요)", cat: "의무·허가",
        conn: "동사·형용사 ない형 어간 + なくてもいい",
        point: "유사표현: ~なくてもかまわない (090번)",
        ex: [
          { jp: "わからなければ <ruby>書<rt>か</rt></ruby>かなくても いいです。", tts: "わからなければ書かなくてもいいです。", ko: "모르면 쓰지 않아도 됩니다." },
          { jp: "あしたは <ruby>来<rt>こ</rt></ruby>なくても いいですよ。", tts: "あしたは来なくてもいいですよ。", ko: "내일은 오지 않아도 돼요." }
        ]
      },
      {
        id: "090", title: "~なくてもかまわない", mean: "~하지 않아도 상관없다", cat: "의무·허가",
        conn: "동사·형용사 ない형 어간 + なくてもかまわない",
        point: "~なくてもいい와 같은 뜻으로, 의무가 없음을 정중하게 허용할 때 사용",
        ex: [
          { jp: "<ruby>飲<rt>の</rt></ruby>めないのなら、むりに <ruby>飲<rt>の</rt></ruby>まなくても かまいません。", tts: "飲めないのなら、むりに飲まなくてもかまいません。", ko: "마실 수 없다면 억지로 마시지 않아도 상관없습니다." },
          { jp: "ぜんぶ <ruby>食<rt>た</rt></ruby>べなくても かまいません。", tts: "ぜんぶ食べなくてもかまいません。", ko: "전부 먹지 않아도 상관없습니다." }
        ]
      },
      {
        id: "091", title: "~なさい", mean: "~해라, ~하세요 (부드러운 명령)", cat: "의무·허가",
        conn: "동사 ます형 + なさい",
        point: "부모가 자녀에게, 선생님이 학생에게, 또는 시험 문제 지시문에서 주로 쓰는 명령 표현",
        ex: [
          { jp: "もう おそいから、はやく <ruby>寝<rt>ね</rt></ruby>なさい。", tts: "もうおそいから、はやく寝なさい。", ko: "벌써 늦었으니까 빨리 자거라." },
          { jp: "つぎの しつもんに <ruby>答<rt>こた</rt></ruby>えなさい。", tts: "つぎのしつもんに答えなさい。", ko: "다음 질문에 답하시오." }
        ]
      },
      {
        id: "092", title: "~ながら", mean: "~하면서 (동시 동작)", cat: "일반문형",
        conn: "동사 ます형 + ながら",
        point: "한 주체가 두 가지 동작을 동시에 행할 때 쓰며, 뒤쪽 동사가 주된 동작임!",
        ex: [
          { jp: "<ruby>音楽<rt>おんがく</rt></ruby>を <ruby>聞<rt>き</rt></ruby>きながら <ruby>勉強<rt>べんきょう</rt></ruby>します。", tts: "音楽を聞きながら勉強します。", ko: "음악을 들으면서 공부합니다." },
          { jp: "お<ruby>茶<rt>ちゃ</rt></ruby>を <ruby>飲<rt>の</rt></ruby>みながら <ruby>話<rt>はな</rt></ruby>しましょう。", tts: "お茶を飲みながら話しましょう。", ko: "차를 마시면서 이야기합시다." }
        ]
      },
      {
        id: "093", title: "~なら", mean: "~(이)라면 (가정·화제 제시)", cat: "가정·조건",
        conn: "동사·형용사·명사 보통형(명사·な형용사 だ 생략) + なら",
        point: "상대방이 꺼낸 말을 받아 가정하거나 조언·제안을 할 때 가장 자연스러운 조건 표현",
        ex: [
          { jp: "<ruby>京都<rt>きょうと</rt></ruby>へ <ruby>行<rt>い</rt></ruby>くなら、<ruby>秋<rt>あき</rt></ruby>が いいですよ。", tts: "京都へ行くなら、秋がいいですよ。", ko: "교토에 간다면 가을이 좋아요." },
          { jp: "やすいなら <ruby>買<rt>か</rt></ruby>いたいです。", tts: "やすいなら買いたいです。", ko: "싸다면 사고 싶습니다." }
        ]
      },
      {
        id: "094", title: "~にする", mean: "~로 (정)하다, 선택하다", cat: "의지·희망",
        conn: "명사 + にする",
        point: "메뉴를 주문하거나 여러 선택지 중 하나를 결정할 때 사용 (비교: 041번 동사+ことにする)",
        ex: [
          { jp: "プレゼントは ネクタイに しましょう。", tts: "プレゼントはネクタイにしましょう。", ko: "선물은 넥타이로 합시다." },
          { jp: "わたしは コーヒーに します。", tts: "わたしはコーヒーにします。", ko: "저는 커피로 하겠습니다." }
        ]
      },
      {
        id: "095", title: "~について", mean: "~에 관해서, ~에 대하여", cat: "일반문형",
        conn: "명사 + について (뒤에 명사를 수식할 때는 ~についての + 명사)",
        point: "글, 이야기, 연구, 생각 등의 주제·대상을 나타낼 때 사용",
        ex: [
          { jp: "<ruby>新聞<rt>しんぶん</rt></ruby>は その <ruby>地震<rt>じしん</rt></ruby>に ついて <ruby>長<rt>なが</rt></ruby>い <ruby>記事<rt>きじ</rt></ruby>を <ruby>載<rt>の</rt></ruby>せた。", tts: "新聞はその地震について長い記事を載せた。", ko: "신문은 그 지진에 관해 긴 기사를 실었다." },
          { jp: "<ruby>池田<rt>いけだ</rt></ruby>さんは <ruby>鳥<rt>とり</rt></ruby>に ついての <ruby>講演<rt>こうえん</rt></ruby>を しました。", tts: "池田さんは鳥についての講演をしました。", ko: "이케다 씨는 새에 관한 강연을 했습니다." }
        ]
      },
      {
        id: "096", title: "~にくい", mean: "~하기 어렵다, ~하기 힘들다", cat: "일반문형",
        conn: "동사 ます형 + にくい",
        point: "반대 표현: ~やすい (~하기 쉽다, 106번) — 전체가 い형용사로 활용함",
        ex: [
          { jp: "この くすりは にがくて <ruby>飲<rt>の</rt></ruby>みにくいです。", tts: "このくすりはにがくて飲みにくいです。", ko: "이 약은 써서 먹기(마시기) 힘듭니다." },
          { jp: "この ペンは <ruby>書<rt>か</rt></ruby>きにくいですね。", tts: "このペンは書きにくいですね。", ko: "이 펜은 쓰기 불편하네요(힘드네요)." }
        ]
      },
      {
        id: "097", title: "~のなかで", mean: "~중에서 (비교 범위)", cat: "일반문형",
        conn: "명사(집단·범위) + のなかで",
        point: "세 개 이상의 범위 중에서 최고(いちばん)를 고를 때 자주 쓰임",
        ex: [
          { jp: "くだものの なかで いちごが いちばん <ruby>好<rt>す</rt></ruby>きです。", tts: "くだもののなかでいちごがいちばん好きです。", ko: "과일 중에서 딸기를 가장 좋아합니다." },
          { jp: "スポーツの なかで なにが いちばん おもしろいですか。", tts: "スポーツのなかでなにがいちばんおもしろいですか。", ko: "스포츠 중에서 무엇이 가장 재미있습니까?" }
        ]
      },
      {
        id: "098", title: "~ば", mean: "~하면 (가정 조건)", cat: "가정·조건",
        conn: "동사 ば형(e단+ば) / い형용사 어간+ければ / な형용사·명사+なら(であれば)",
        point: "일반적·논리적 조건이나 속담 등에 자주 사용 (예: ば~ほど ~하면 할수록)",
        ex: [
          { jp: "<ruby>春<rt>はる</rt></ruby>に なれば、あたたかく なります。", tts: "春になれば、あたたかくなります。", ko: "봄이 되면 따뜻해집니다." },
          { jp: "やすければ、あの パソコンを <ruby>買<rt>か</rt></ruby>います。", tts: "やすければ、あのパソコンを買います。", ko: "싸다면 저 컴퓨터를 사겠습니다." }
        ]
      },
      {
        id: "099", title: "~ばいい", mean: "~하면 된다, ~하면 좋다 (조언·희망)", cat: "가정·조건",
        conn: "동사·형용사 ば형 + いい (과거형+らいい / ~たらいい와 유사)",
        point: "상대방에게 해결책을 조언하거나 의문사와 함께 '어떻게 하면 좋을지(どうすればいいか)' 물을 때 사용",
        ex: [
          { jp: "わからない ときは、<ruby>先生<rt>せんせい</rt></ruby>に <ruby>聞<rt>き</rt></ruby>けば いいですよ。", tts: "わからないときは、先生に聞けばいいですよ。", ko: "모를 때는 선생님께 물어보면 돼요." },
          { jp: "どこで きっぷを <ruby>買<rt>か</rt></ruby>えば いいですか。", tts: "どこできっぷを買えばいいですか。", ko: "어디서 표를 사면 됩니까?" }
        ]
      },
      {
        id: "100", title: "~はずだ", mean: "~할 것이다, ~할 리가 있다 (당연한 추측·확신)", cat: "추측·전문",
        conn: "동사·い형용사 보통형 / な형용사+な / 명사+の + はずだ",
        point: "객관적 근거를 바탕으로 '틀림없이 ~할 것이다'라고 확신할 때 사용 (부정: ~はずがない ~할 리가 없다)",
        ex: [
          { jp: "かれは <ruby>日本<rt>にほん</rt></ruby>に 5<ruby>年<rt>ねん</rt></ruby> いたから、<ruby>日本語<rt>にほんご</rt></ruby>が <ruby>上手<rt>じょうず</rt></ruby>な はずです。", tts: "かれは日本に5年いたから、日本語が上手なはずです。", ko: "그는 일본에 5년 있었으니까 일본어를 잘할 것입니다." },
          { jp: "きのう おくったから、きょう <ruby>届<rt>とど</rt></ruby>く はずだ。", tts: "きのうおくったから、きょう届くはずだ。", ko: "어제 보냈으니까 오늘 도착할 것이다." }
        ]
      },
      {
        id: "101", title: "~はじめる", mean: "~하기 시작하다", cat: "일반문형",
        conn: "동사 ます형 + はじめる",
        point: "복합동사로 2그룹 활용을 함 (비교: 갑작스러운 시작은 ~だす 053번)",
        ex: [
          { jp: "わたしは <ruby>先月<rt>せんげつ</rt></ruby>から <ruby>日本語<rt>にほんご</rt></ruby>を ならいはじめました。", tts: "わたしは先月から日本語をならいはじめました。", ko: "나는 지난달부터 일본어를 배우기 시작했습니다." },
          { jp: "さくらが さきはじめました。", tts: "さくらがさきはじめました。", ko: "벚꽃이 피기 시작했습니다." }
        ]
      },
      {
        id: "102", title: "~ほしい / ~てほしい", mean: "(명사를) 갖고 싶다 / (남이) ~해 주길 바란다", cat: "의지·희망",
        conn: "명사 + が ほしい / 동사 て형 + ほしい (부정: ~ないでほしい)",
        point: "~たい는 내가 하고 싶은 것, ~てほしい는 상대방이 해 주기를 바라는 것!",
        ex: [
          { jp: "わたしは あたらしい <ruby>車<rt>くるま</rt></ruby>が ほしいです。", tts: "わたしはあたらしい車がほしいです。", ko: "나는 새 차를 갖고 싶습니다." },
          { jp: "あなたに ほんとうの ことを <ruby>言<rt>い</rt></ruby>って ほしい。", tts: "あなたにほんとうのことを言ってほしい。", ko: "당신이 진실을 말해 주길 바랍니다." }
        ]
      },
      {
        id: "103", title: "~ほど", mean: "~만큼, ~정도 (비교·정도)", cat: "일반문형",
        conn: "명사·동사 보통형 + ほど (주로 부정문 ~ほど ~ない: ~만큼 ~하지 않다)",
        point: "'Aは Bほど ~ない(A는 B만큼 ~하지 않다 = B가 더 ~하다)' 비교 구문으로 자주 출제!",
        ex: [
          { jp: "ことしの <ruby>夏<rt>なつ</rt></ruby>は きょねんほど あつく ない。", tts: "ことしの夏はきょねんほどあつくない。", ko: "올여름은 작년만큼 덥지 않다." },
          { jp: "<ruby>歩<rt>ある</rt></ruby>けないほど つかれました。", tts: "歩けないほどつかれました。", ko: "걸을 수 없을 정도로 피곤했습니다." }
        ]
      },
      {
        id: "104", title: "~ほうがいい", mean: "~하는 편이 좋다 (조언·권유)", cat: "일반문형",
        conn: "동사 た형(과거형) / ない형 + ほうがいい",
        point: "긍정 조언일 때는 동사 과거형(た형)을 쓰고, 부정 조언(~하지 않는 편이 좋다)일 때는 ない형을 씀!",
        ex: [
          { jp: "かぜを ひいた ときは、はやく <ruby>寝<rt>ね</rt></ruby>た ほうが いいですよ。", tts: "かぜをひいたときは、はやく寝たほうがいいですよ。", ko: "감기에 걸렸을 때는 일찍 자는 편이 좋아요." },
          { jp: "おさけは あまり <ruby>飲<rt>の</rt></ruby>まない ほうが いいです。", tts: "おさけはあまり飲まないほうがいいです。", ko: "술은 그다지 마시지 않는 편이 좋습니다." }
        ]
      },
      {
        id: "105", title: "~마え(前)に", mean: "~하기 전에", cat: "일반문형",
        conn: "동사 사전형 / 명사の / 시간 수량사 + まえに",
        point: "뒷문장이 과거 시제여도 まえに 앞의 동사는 무조건 사전형(기본형) 고정! (070번 연계)",
        ex: [
          { jp: "<ruby>寝<rt>ね</rt></ruby>る <ruby>前<rt>まえ</rt></ruby>に はを みがきます。", tts: "寝る前にはをみがきます。", ko: "자기 전에 이를 닦습니다." },
          { jp: "<ruby>食事<rt>しょくじ</rt></ruby>の <ruby>前<rt>まえ</rt></ruby>に <ruby>手<rt>て</rt></ruby>を あらいなさい。", tts: "食事の前に手をあらいなさい。", ko: "식사 전에 손을 씻으렴." }
        ]
      },
      {
        id: "106", title: "~やすい", mean: "~하기 쉽다, ~하기 편하다", cat: "일반문형",
        conn: "동사 ます형 + やすい",
        point: "반대 표현: ~にくい (~하기 어렵다/불편하다, 096번)",
        ex: [
          { jp: "これは かるくて はきやすい くつです。", tts: "これはかるくてはきやすいくつです。", ko: "이것은 가볍고 신기 편한 신발입니다." },
          { jp: "<ruby>田中先生<rt>たなかせんせい</rt></ruby>の せつめいは わかりやすいです。", tts: "田中先生のせつめいはわかりやすいです。", ko: "다나카 선생님의 설명은 알기 쉽습니다." }
        ]
      },
      {
        id: "107", title: "~ようだ", mean: "~인 것 같다 (추량·비유·예시)", cat: "추측·전문",
        conn: "동사·い형용사 보통형 / な형용사+な / 명사+の + ようだ",
        point: "[추량] どうも~ようだ(아무래도 ~인 것 같다) / [비유] まるで~ようだ(마치 ~같다) / [예시] たとえば~のような",
        ex: [
          { jp: "<ruby>風<rt>かぜ</rt></ruby>が つめたいです。<ruby>今日<rt>きょう</rt></ruby>は きのうより さむいようです。", tts: "風がつめたいです。今日はきのうよりさむいようです。", ko: "바람이 찹니다. 오늘은 어제보다 추운 것 같습니다. (추량)" },
          { jp: "かれは どうも かぜを ひいたようです。", tts: "かれはどうもかぜをひいたようです。", ko: "그는 아무래도 감기에 걸린 것 같습니다." }
        ]
      },
      {
        id: "108", title: "~ように", mean: "~하도록, ~하기 위해서 (목적·기원·당부)", cat: "일반문형",
        conn: "동사 사전형(주로 가능동사·무의지동사) / ない형 + ように",
        point: "목적을 나타낼 때 ために(056) 앞에는 의지동사, ように 앞에는 가능동사·무의지동사·부정형(~ないように)이 옴!",
        ex: [
          { jp: "<ruby>忘<rt>わす</rt></ruby>れないように メモを して おきます。", tts: "忘れないようにメモをしておきます。", ko: "잊어버리지 않도록 메모를 해 둡니다." },
          { jp: "うしろの <ruby>人<rt>ひと</rt></ruby>にも <ruby>聞<rt>き</rt></ruby>こえるように <ruby>大<rt>おお</rt></ruby>きな <ruby>声<rt>こえ</rt></ruby>で <ruby>話<rt>はな</rt></ruby>した。", tts: "うしろの人にも聞こえるように大きな声で話した。", ko: "뒤쪽 사람에게도 들리도록 큰 소리로 말했다." }
        ]
      },
      {
        id: "109", title: "~ようにする / ~ようになる", mean: "~하도록 하다(노력) / ~하게끔 되다(변화)", cat: "일반문형",
        conn: "동사 사전형·ない형 + ようにする / 가능동사·사전형·ない형 + ようになる",
        point: "ようにする는 습관적 노력·결심, ようになる는 능력이나 상태의 변화(전에 못하던 것을 하게 됨)!",
        ex: [
          { jp: "あしたは ねぼうしないように します。", tts: "あしたはねぼうしないようにします。", ko: "내일은 늦잠 안 자도록 할게요." },
          { jp: "さいきん <ruby>運転<rt>うんてん</rt></ruby>できるように なりました。", tts: "さいきん運転できるようになりました。", ko: "최근에 운전할 수 있게 되었습니다." }
        ]
      },
      {
        id: "110", title: "~より~ほう(が)", mean: "~보다 ~쪽(이) 더 ...", cat: "일반문형",
        conn: "명사/보통형 + より + 명사の/보통형 + ほうが",
        point: "'Aより Bのほうが ~(A보다 B 쪽이 더 ~하다)' 비교 구문 (관련표현: 083번 ~と~とどちら)",
        ex: [
          { jp: "あの <ruby>店<rt>みせ</rt></ruby>より この <ruby>店<rt>みせ</rt></ruby>の ほうが いいです。", tts: "あの店よりこの店のほうがいいです。", ko: "저 가게보다 이 가게 쪽이(가게가) 좋아요." },
          { jp: "バスより <ruby>電車<rt>でんしゃ</rt></ruby>の ほうが はやいです。", tts: "バスより電車のほうがはやいです。", ko: "버스보다 전철 쪽이 더 빠릅니다." }
        ]
      },
      {
        id: "111", title: "~らしい", mean: "~인 것 같다(객관적 추측), ~답다(전형적 속성)", cat: "추측·전문",
        conn: "동사·형용사·명사 보통형(명사·な형용사 だ 생략) + らしい",
        point: "소문이나 객관적 근거에 의한 추측(~인 모양이다) 또는 명사에 바로 붙어 성질 강조(学生らしい 학생답다)",
        ex: [
          { jp: "てんきよほうに よると、あしたは <ruby>雨<rt>あめ</rt></ruby>らしい。", tts: "てんきよほうによると、あしたは雨らしい。", ko: "일기예보에 의하면 내일은 비가 올 것 같다." },
          { jp: "きょうは <ruby>春<rt>はる</rt></ruby>らしい あたたかい <ruby>日<rt>ひ</rt></ruby>ですね。", tts: "きょうは春らしいあたたかい日ですね。", ko: "오늘은 봄다운 따뜻한 날이네요." }
        ]
      },
      {
        id: "112", title: "~(ら)れる (수동·존경)", mean: "~당하다/~되다(수동), ~하시다(존경)", cat: "사역·수동",
        conn: "1그룹: ない형 어간+れる / 2그룹: ない형 어간+られる / する➔される, 来る➔こられる",
        point: "수동문과 존경어의 형태가 동일하므로 문맥과 조사(행위자 に)로 구별!",
        ex: [
          { jp: "わたしは <ruby>母<rt>はは</rt></ruby>に にっきを <ruby>見<rt>み</rt></ruby>られました。", tts: "わたしは母ににっきを見られました。", ko: "나는 어머니에게 일기를 들켰습니다(보임을 당했습니다). (수동)" },
          { jp: "<ruby>先生<rt>せんせい</rt></ruby>は なんじに <ruby>来<rt>こ</rt></ruby>られますか。", tts: "先生はなんじに来られますか。", ko: "선생님은 몇 시에 오십니까? (존경)" }
        ]
      },
      {
        id: "113", title: "~(ら)れる (가능동사)", mean: "~할 수 있다 (가능)", cat: "사역·수동",
        conn: "1그룹: e단+る(行く➔行ける) / 2그룹: 어간+られる(食べる➔食べられる) / する➔できる, 来る➔こられる",
        point: "가능동사 앞의 목적격 조사 を는 주로 が로 바뀜! (~ことができる 040번과 동일 의미)",
        ex: [
          { jp: "わたしは さしみが <ruby>食<rt>た</rt></ruby>べられません。", tts: "わたしはさしみが食べられません。", ko: "나는 회를 먹을 수 없습니다." },
          { jp: "<ruby>漢字<rt>かんじ</rt></ruby>で <ruby>名前<rt>なまえ</rt></ruby>が <ruby>書<rt>か</rt></ruby>けますか。", tts: "漢字で名前が書けますか。", ko: "한자로 이름을 쓸 수 있습니까?" }
        ]
      },
      {
        id: "114", title: "명령형 (~해라)", mean: "~해라 (강한 명령)", cat: "의무·허가",
        conn: "1그룹: e단(行く➔行け) / 2그룹: 어간+ろ(起きる➔起きろ) / する➔しろ, 来る➔こい",
        point: "구어체에서 강한 명령이나 응원, 비상시 지시에 사용 (유사표현: ~なさい 091번)",
        ex: [
          { jp: "<ruby>早<rt>はや</rt></ruby>く <ruby>起<rt>お</rt></ruby>きろ。<ruby>会社<rt>かいしゃ</rt></ruby>に おくれるよ。", tts: "早く起きろ。会社におくれるよ。", ko: "빨리 일어나. 회사에 늦겠어." },
          { jp: "あきらめずに さいごまで がんばれ！", tts: "あきらめずにさいごまでがんばれ！", ko: "포기하지 말고 끝까지 힘내라!" }
        ]
      }
    ];

    // ==========================================
    // 4. 실전 퀴즈 데이터 (3단계에서 20문항으로 최종 확장 예정)
    // ==========================================
    const quizData = [
      {
        q: "パーティーは 何時に 始まる（  ） 知って いますか。",
        tts: "パーティーは何時に始まるか知っていますか。",
        ko: "파티는 몇 시에 시작되는지 알고 있습니까?",
        options: ["か", "が", "を", "で"],
        answer: 0,
        exp: "의문사절을 문장 안에 포함시킬 때는 '의문사 + か + 知っていますか/分かりますか' 형태를 씁니다. (001번)"
      },
      {
        q: "今 持っている おかねは 千円くらい（  ） ない。",
        tts: "今持っているおかねは千円くらいしかない。",
        ko: "지금 갖고 있는 돈은 천 엔 정도밖에 없다.",
        options: ["だけ", "しか", "ばかり", "ほど"],
        answer: 1,
        exp: "뒤에 부정어(ない)와 호응하여 '~밖에'를 나타내는 조사는 'しか'입니다. (006번)"
      },
      {
        q: "その 公園は しずかで きれい（  ）、よく さんぽに 行きます。",
        tts: "その公園はしずかできれいなので、よくさんぽに行きます。",
        ko: "그 공원은 조용하고 깨끗하기 때문에 자주 산책하러 갑니다.",
        options: ["ので", "なので", "だから", "のに"],
        answer: 1,
        exp: "な형용사(きれいだ)나 명사 뒤에 '~이므로(ので)'가 접속할 때는 'なので' 형태가 됩니다. (015번)"
      },
      {
        q: "かれは 子ども（  ） タバコを すって いる。",
        tts: "かれは子どもなのにタバコをすっている。",
        ko: "그는 어린이인데도 담배를 피우고 있다.",
        options: ["ので", "なのに", "だから", "ために"],
        answer: 1,
        exp: "명사(子ども) 뒤에 역접의 '~인데도(のに)'가 붙을 때는 'なのに'가 됩니다. (016번)"
      },
      {
        q: "あしたは あさ 9時（  ） わたしの ところに 来て ください。",
        tts: "あしたはあさ9時までにわたしのところに来てください。",
        ko: "내일은 아침 9시까지 나한테 와 주세요.",
        options: ["まで", "までに", "あいだ", "ごろ"],
        answer: 1,
        exp: "일회성 동작(오다)이 완료되어야 하는 마감 시점·기한을 나타낼 때는 'までに'를 씁니다. (019번)"
      },
      {
        q: "この 紙に 住所と お名前を （  ） ください。",
        tts: "この紙に住所とお名前をお書きください。",
        ko: "이 종이에 주소와 성함을 적어 주세요.",
        options: ["書く", "書いて", "お書き", "書こう"],
        answer: 2,
        exp: "'お + 동사 ます형 + ください'는 상대방에게 정중하게 요청하는 존경 표현입니다. (027번)"
      },
      {
        q: "社長は、いま 新聞を お読みに （  ） います。",
        tts: "社長は、いま新聞をお読みになっています。",
        ko: "사장님은 지금 신문을 읽고 계십니다.",
        options: ["して", "なって", "くださって", "いただいて"],
        answer: 1,
        exp: "윗사람의 행동을 높이는 존경어는 'お + 동사 ます형 + になる'입니다. (029번)"
      },
      {
        q: "わたしは 毎日 ジョギングを することに （  ）。",
        tts: "わたしは毎日ジョギングをすることにしました。",
        ko: "나는 매일 조깅을 하기로 했습니다.",
        options: ["なりました", "しました", "できました", "ありました"],
        answer: 1,
        exp: "화자 자신의 의지로 '~하기로 하다(결정)'를 나타낼 때는 'ことにする'를 씁니다. (041번)"
      },
      {
        q: "この かばんは （  ）すぎて ふべんです。",
        tts: "このかばんは大きすぎてふべんです。",
        ko: "이 가방은 너무 커서 불편합니다.",
        options: ["大きい", "大きく", "大き", "大きな"],
        answer: 2,
        exp: "'너무 ~하다(~すぎる)'는 형용사의 어간(大きい ➔ 大き)이나 동사 ます형에 접속합니다. (045번)"
      },
      {
        q: "ニュースに よると、きのう 神戸で じしんが （  ）そうです。",
        tts: "ニュースによると、きのう神戸でじしんがあったそうです。",
        ko: "뉴스에 따르면 어제 고베에서 지진이 있었다고 합니다.",
        options: ["あり", "あって", "あった", "ある"],
        answer: 2,
        exp: "'~에 의하면(~によると)'과 호응하는 전문(~라고 한다)의 そうだ 앞에는 보통형(과거형 あった)이 접속합니다. (050번)"
      }
    ];

    // ==========================================
    // 5. 상태 관리 및 로컬 스토리지
    // ==========================================
    let currentCategory = 'ALL';
    let showBookmarksOnly = false;
    let isMemoryMode = false;
    let bookmarks = JSON.parse(localStorage.getItem('jlpt_n4_bookmarks') || '[]');
    let wrongAnswers = JSON.parse(localStorage.getItem('jlpt_n4_wrong_quiz') || '[]');

    // 퀴즈 진행 상태
    let activeQuizList = [];
    let currentQuizIndex = 0;
    let score = 0;
    let isRetryMode = false;

    // ==========================================
    // 6. 핵심 기능 함수 (TTS, 다크모드, 암기모드, 렌더링)
    // ==========================================
    function speakJP(text) {
      if (!('speechSynthesis' in window)) {
        alert('이 브라우저는 음성 재생(TTS)을 지원하지 않습니다.');
        return;
      }
      window.speechSynthesis.cancel();
      const utter = new SpeechSynthesisUtterance(text);
      utter.lang = 'ja-JP';
      utter.rate = 0.92;
      window.speechSynthesis.speak(utter);
    }

    function toggleDarkMode() {
      const html = document.documentElement;
      const isDark = html.classList.toggle('dark');
      document.getElementById('darkModeIcon').textContent = isDark ? '☀️' : '🌙';
      localStorage.setItem('jlpt_n4_theme', isDark ? 'dark' : 'light');
    }

    function toggleMemoryMode() {
      isMemoryMode = !isMemoryMode;
      document.body.classList.toggle('memory-mode', isMemoryMode);
      const btnText = document.getElementById('memoryModeText');
      const btn = document.getElementById('memoryModeBtn');
      if (isMemoryMode) {
        btnText.textContent = '암기 모드 ON';
        btn.classList.add('bg-brand-500', 'text-white');
        btn.classList.remove('bg-slate-100', 'dark:bg-slate-700');
      } else {
        btnText.textContent = '암기 모드 OFF';
        btn.classList.remove('bg-brand-500', 'text-white');
        btn.classList.add('bg-slate-100', 'dark:bg-slate-700');
        document.querySelectorAll('.blur-target.revealed').forEach(el => el.classList.remove('revealed'));
      }
    }

    function switchTab(tabName) {
      ['dict', 'conj', 'tips', 'quiz'].forEach(t => {
        document.getElementById(`section-${t}`).classList.toggle('hidden', t !== tabName);
        const btn = document.getElementById(`tab-${t}`);
        if (t === tabName) {
          btn.className = "tab-btn px-4 py-2.5 text-sm font-bold border-b-2 border-brand-500 text-brand-600 dark:text-brand-400 whitespace-nowrap";
        } else {
          btn.className = "tab-btn px-4 py-2.5 text-sm font-medium border-b-2 border-transparent text-slate-500 hover:text-slate-800 dark:text-slate-400 dark:hover:text-slate-200 whitespace-nowrap";
        }
      });
      if (tabName === 'quiz' && activeQuizList.length === 0) {
        startQuiz(false);
      }
    }

    function setCategory(cat) {
      currentCategory = cat;
      document.querySelectorAll('.cat-btn').forEach(btn => {
        if (btn.dataset.cat === cat) {
          btn.className = "cat-btn px-3 py-1 rounded-full text-xs font-bold bg-brand-500 text-white";
        } else {
          btn.className = "cat-btn px-3 py-1 rounded-full text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300 hover:bg-slate-200";
        }
      });
      renderGrammarList();
    }

    function toggleBookmarkFilter() {
      showBookmarksOnly = !showBookmarksOnly;
      const btn = document.getElementById('bookmarkFilterBtn');
      if (showBookmarksOnly) {
        btn.classList.add('bg-amber-100', 'border-amber-400', 'text-amber-800', 'dark:bg-amber-900/40', 'dark:text-amber-200');
      } else {
        btn.classList.remove('bg-amber-100', 'border-amber-400', 'text-amber-800', 'dark:bg-amber-900/40', 'dark:text-amber-200');
      }
      renderGrammarList();
    }

    function toggleBookmark(id) {
      if (bookmarks.includes(id)) {
        bookmarks = bookmarks.filter(b => b !== id);
      } else {
        bookmarks.push(id);
      }
      localStorage.setItem('jlpt_n4_bookmarks', JSON.stringify(bookmarks));
      document.getElementById('bookmarkCount').textContent = bookmarks.length;
      renderGrammarList();
    }

    function renderGrammarList() {
      const query = document.getElementById('searchInput').value.trim().toLowerCase();
      const grid = document.getElementById('grammarGrid');
      document.getElementById('grammarTotalCount').textContent = grammarData.length;
      document.getElementById('bookmarkCount').textContent = bookmarks.length;

      const filtered = grammarData.filter(item => {
        const matchCat = currentCategory === 'ALL' || item.cat === currentCategory;
        const matchBm = !showBookmarksOnly || bookmarks.includes(item.id);
        const matchQuery = !query ||
          item.id.includes(query) ||
          item.title.toLowerCase().includes(query) ||
          item.mean.toLowerCase().includes(query) ||
          item.point.toLowerCase().includes(query);
        return matchCat && matchBm && matchQuery;
      });

      if (filtered.length === 0) {
        grid.innerHTML = `<div class="col-span-full text-center py-12 text-slate-400">조건에 맞는 문법 항목이 없습니다.</div>`;
        return;
      }

      grid.innerHTML = filtered.map(item => {
        const isBm = bookmarks.includes(item.id);
        return `
          <article class="bg-white dark:bg-slate-800 rounded-xl shadow-sm border border-slate-200 dark:border-slate-700 p-5 flex flex-col justify-between space-y-4 hover:border-brand-500/50 transition">
            <div class="space-y-3">
              <!-- 카드 상단 헤더 -->
              <div class="flex items-start justify-between gap-2">
                <div>
                  <div class="flex items-center gap-2 mb-1">
                    <span class="px-2 py-0.5 rounded text-xs font-bold bg-brand-100 text-brand-700 dark:bg-brand-700 dark:text-white">#${item.id}</span>
                    <span class="px-2 py-0.5 rounded text-xs font-medium bg-slate-100 dark:bg-slate-700 text-slate-600 dark:text-slate-300">${item.cat}</span>
                  </div>
                  <h3 class="text-lg font-bold jp-text text-slate-900 dark:text-white">${item.title}</h3>
                </div>
                <button onclick="toggleBookmark('${item.id}')" class="text-xl leading-none p-1" title="북마크">
                  ${isBm ? '⭐' : '☆'}
                </button>
              </div>

              <!-- 핵심 한국어 뜻 (암기 모드 블러 대상) -->
              <p onclick="this.classList.toggle('revealed')" class="blur-target text-base font-bold text-brand-600 dark:text-brand-400">${item.mean}</p>

              <!-- 접속 형태 & 포인트 뱃지 -->
              <div class="text-xs space-y-1.5 bg-slate-50 dark:bg-slate-900/60 p-3 rounded-lg border border-slate-100 dark:border-slate-700/60">
                <div><strong class="text-slate-700 dark:text-slate-300">🔗 접속:</strong> <span class="text-slate-600 dark:text-slate-400">${item.conn}</span></div>
                <div><strong class="text-brand-600 dark:text-brand-400">💡 포인트:</strong> <span class="text-slate-600 dark:text-slate-400">${item.point}</span></div>
              </div>

              <!-- 예문 2가지 -->
              <div class="space-y-2.5 pt-1">
                ${item.ex.map(e => `
                  <div class="p-3 rounded-lg bg-slate-50/70 dark:bg-slate-700/30 border border-slate-200/60 dark:border-slate-700">
                    <div class="flex items-start justify-between gap-2">
                      <p class="jp-text text-sm md:text-base leading-relaxed font-medium">${e.jp}</p>
                      <button onclick="speakJP('${e.tts.replace(/'/g, "\\'")}')" class="p-1.5 rounded-md bg-white dark:bg-slate-700 hover:bg-brand-50 dark:hover:bg-slate-600 border border-slate-200 dark:border-slate-600 text-xs shrink-0" title="발음 듣기">🔊</button>
                    </div>
                    <p onclick="this.classList.toggle('revealed')" class="blur-target text-xs md:text-sm text-slate-500 dark:text-slate-400 mt-1">${e.ko}</p>
                  </div>
                `).join('')}
              </div>
            </div>
          </article>
        `;
      }).join('');
    }

    function renderConjunctionsAndAdverbs() {
      const conjGrid = document.getElementById('conjGrid');
      conjGrid.innerHTML = conjunctionsData.map(group => `
        <div class="bg-white dark:bg-slate-800 rounded-xl p-4 shadow-sm border border-slate-200 dark:border-slate-700 space-y-3">
          <h3 class="font-bold text-brand-600 dark:text-brand-400 border-b border-slate-100 dark:border-slate-700 pb-2">${group.type}</h3>
          <div class="space-y-2.5">
            ${group.items.map(it => `
              <div class="p-2.5 rounded-lg bg-slate-50 dark:bg-slate-900/50 text-sm space-y-1">
                <div class="flex items-center justify-between">
                  <span class="font-bold jp-text">${it.jp}</span>
                  <button onclick="speakJP('${it.ex}')" class="text-xs px-1.5 py-0.5 rounded bg-slate-200 dark:bg-slate-700">🔊 예문</button>
                </div>
                <p onclick="this.classList.toggle('revealed')" class="blur-target text-xs font-medium text-slate-700 dark:text-slate-300">${it.ko}</p>
                <p class="text-xs text-slate-500 dark:text-slate-400 jp-text">${it.ex}</p>
              </div>
            `).join('')}
          </div>
        </div>
      `).join('');

      const advGrid = document.getElementById('advGrid');
      advGrid.innerHTML = adverbsData.map(group => `
        <div class="bg-white dark:bg-slate-800 rounded-xl p-4 shadow-sm border border-slate-200 dark:border-slate-700 space-y-3">
          <h3 class="font-bold text-brand-600 dark:text-brand-400 border-b border-slate-100 dark:border-slate-700 pb-2">${group.category}</h3>
          <div class="space-y-2.5">
            ${group.items.map(it => `
              <div class="p-2.5 rounded-lg bg-slate-50 dark:bg-slate-900/50 text-sm space-y-1">
                <div class="flex items-center justify-between">
                  <span class="font-bold jp-text text-brand-600 dark:text-brand-400">${it.jp}</span>
                  <button onclick="speakJP('${it.ex}')" class="text-xs px-1.5 py-0.5 rounded bg-slate-200 dark:bg-slate-700">🔊 듣기</button>
                </div>
                <p onclick="this.classList.toggle('revealed')" class="blur-target text-xs font-medium text-slate-700 dark:text-slate-300">${it.ko}</p>
                <p class="text-xs text-slate-500 dark:text-slate-400 jp-text">${it.ex}</p>
              </div>
            `).join('')}
          </div>
        </div>
      `).join('');
    }

    function renderComparisonTips() {
      const container = document.getElementById('tipsContainer');
      container.innerHTML = comparisonTips.map(tip => `
        <div class="bg-white dark:bg-slate-800 rounded-xl p-5 shadow-sm border border-slate-200 dark:border-slate-700 space-y-3 overflow-x-auto">
          <h3 class="font-bold text-base md:text-lg text-slate-900 dark:text-white">${tip.title}</h3>
          <table class="w-full text-left border-collapse text-xs md:text-sm">
            <thead>
              <tr class="bg-slate-100 dark:bg-slate-700/70 text-slate-700 dark:text-slate-200">
                ${tip.headers.map(h => `<th class="p-2.5 border border-slate-200 dark:border-slate-600 font-bold">${h}</th>`).join('')}
              </tr>
            </thead>
            <tbody>
              ${tip.rows.map(row => `
                <tr class="hover:bg-slate-50 dark:hover:bg-slate-700/30">
                  ${row.map((cell, idx) => `
                    <td onclick="this.classList.toggle('revealed')" class="p-2.5 border border-slate-200 dark:border-slate-700 ${idx === 0 ? 'font-bold bg-slate-50/50 dark:bg-slate-800' : 'blur-target jp-text'}">${cell}</td>
                  `).join('')}
                </tr>
              `).join('')}
            </tbody>
          </table>
        </div>
      `).join('');
    }

    // ==========================================
    // 7. 퀴즈 및 오답노트 로직
    // ==========================================
    function startQuiz(retryWrongOnly) {
      isRetryMode = retryWrongOnly;
      if (retryWrongOnly) {
        activeQuizList = quizData.filter((_, idx) => wrongAnswers.includes(idx));
        document.getElementById('quizModeBadge').textContent = "오답 복습 모드";
      } else {
        activeQuizList = [...quizData];
        document.getElementById('quizModeBadge').textContent = "전체 실전 퀴즈";
      }
      currentQuizIndex = 0;
      score = 0;
      document.getElementById('quizQuestionBox').classList.remove('hidden');
      document.getElementById('quizResultBox').classList.add('hidden');
      renderQuizQuestion();
    }

    function renderQuizQuestion() {
      const item = activeQuizList[currentQuizIndex];
      document.getElementById('quizProgress').textContent = `문제 ${currentQuizIndex + 1} / ${activeQuizList.length}`;
      document.getElementById('quizQuestion').textContent = item.q;
      document.getElementById('quizKoHint').textContent = `해석: ${item.ko}`;
      document.getElementById('quizTtsBtn').onclick = () => speakJP(item.tts);

      const feedback = document.getElementById('quizFeedback');
      feedback.className = "hidden p-4 rounded-lg text-sm space-y-1";
      document.getElementById('nextQuizBtn').classList.add('hidden');

      const optsBox = document.getElementById('quizOptions');
      optsBox.innerHTML = item.options.map((opt, idx) => `
        <button onclick="checkAnswer(${idx})" class="quiz-opt-btn w-full text-left px-4 py-3 rounded-lg border border-slate-300 dark:border-slate-600 hover:bg-slate-100 dark:hover:bg-slate-700 font-medium jp-text transition flex items-center justify-between">
          <span>${idx + 1}. ${opt}</span>
        </button>
      `).join('');
    }

    function checkAnswer(selectedIdx) {
      const item = activeQuizList[currentQuizIndex];
      const originalQuizIdx = quizData.indexOf(item);
      const isCorrect = selectedIdx === item.answer;

      document.querySelectorAll('.quiz-opt-btn').forEach((btn, idx) => {
        btn.disabled = true;
        if (idx === item.answer) {
          btn.classList.add('bg-emerald-100', 'border-emerald-500', 'text-emerald-900', 'dark:bg-emerald-900/50', 'dark:text-emerald-200', 'font-bold');
        } else if (idx === selectedIdx && !isCorrect) {
          btn.classList.add('bg-rose-100', 'border-rose-500', 'text-rose-900', 'dark:bg-rose-900/50', 'dark:text-rose-200');
        }
      });

      const feedback = document.getElementById('quizFeedback');
      feedback.classList.remove('hidden');

      if (isCorrect) {
        score++;
        feedback.classList.add('bg-emerald-50', 'text-emerald-800', 'dark:bg-emerald-950/60', 'dark:text-emerald-200', 'border', 'border-emerald-200');
        feedback.innerHTML = `<div class="font-bold">⭕ 정답입니다!</div><div>${item.exp}</div>`;
        if (wrongAnswers.includes(originalQuizIdx)) {
          wrongAnswers = wrongAnswers.filter(i => i !== originalQuizIdx);
          localStorage.setItem('jlpt_n4_wrong_quiz', JSON.stringify(wrongAnswers));
        }
      } else {
        feedback.classList.add('bg-rose-50', 'text-rose-800', 'dark:bg-rose-950/60', 'dark:text-rose-200', 'border', 'border-rose-200');
        feedback.innerHTML = `<div class="font-bold">❌ 오답입니다. (정답: ${item.options[item.answer]})</div><div>${item.exp}</div>`;
        if (!wrongAnswers.includes(originalQuizIdx)) {
          wrongAnswers.push(originalQuizIdx);
          localStorage.setItem('jlpt_n4_wrong_quiz', JSON.stringify(wrongAnswers));
        }
      }

      document.getElementById('nextQuizBtn').classList.remove('hidden');
    }

    function nextQuestion() {
      currentQuizIndex++;
      if (currentQuizIndex < activeQuizList.length) {
        renderQuizQuestion();
      } else {
        showQuizResult();
      }
    }

    function showQuizResult() {
      document.getElementById('quizQuestionBox').classList.add('hidden');
      const resultBox = document.getElementById('quizResultBox');
      resultBox.classList.remove('hidden');
      document.getElementById('quizScoreText').textContent = `총 ${activeQuizList.length}문제 중 ${score}문제를 맞혔습니다!`;

      const retryBtn = document.getElementById('retryWrongBtn');
      document.getElementById('wrongCount').textContent = wrongAnswers.length;
      if (wrongAnswers.length > 0) {
        retryBtn.classList.remove('hidden');
      } else {
        retryBtn.classList.add('hidden');
      }
    }

    // 초기화 실행
    window.addEventListener('DOMContentLoaded', () => {
      if (localStorage.getItem('jlpt_n4_theme') === 'dark') {
        document.documentElement.classList.add('dark');
        document.getElementById('darkModeIcon').textContent = '☀️';
      }
      renderGrammarList();
      renderConjunctionsAndAdverbs();
      renderComparisonTips();
    });
  </script>
</body>
</html>
