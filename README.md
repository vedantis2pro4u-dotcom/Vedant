<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no, viewport-fit=cover">
  <meta name="mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">
  <meta name="apple-mobile-web-app-title" content="KisanDirect">
  <meta name="theme-color" content="#1a381e">
  <title>KisanDirect - Kisan Mandi & Live Bidding</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css" rel="stylesheet">
  <link href="https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@400;500;600;700;800&family=Noto+Sans+Devanagari:wght@400;600;700;800&display=swap" rel="stylesheet">
  <style>
    body { 
      font-family: 'Plus Jakarta Sans', 'Noto Sans Devanagari', sans-serif; 
      -webkit-tap-highlight-color: transparent;
      user-select: none;
      -webkit-user-select: none;
      background-color: #f7f4ed;
    }
    .no-scrollbar::-webkit-scrollbar { display: none; }
    .no-scrollbar { -ms-overflow-style: none; scrollbar-width: none; }
    input[type=range]::-webkit-slider-thumb {
      height: 24px; width: 24px; border-radius: 50%; background: #d97706; cursor: pointer; -webkit-appearance: none; box-shadow: 0 2px 8px rgba(0,0,0,0.3); border: 3px solid #fff;
    }
    .farm-pattern {
      background-color: #1e3a24;
      background-image: radial-gradient(#d97706 0.75px, transparent 0.75px), radial-gradient(#2d5a34 0.75px, #1e3a24 0.75px);
      background-size: 30px 30px;
      background-position: 0 0, 15px 15px;
    }
  </style>
</head>
<body class="text-stone-800 antialiased pb-28 min-h-screen">

  <!-- ================= AUTH SCREEN ================= -->
  <div id="authScreen" class="fixed inset-0 z-50 bg-stone-900/80 backdrop-blur-md flex items-center justify-center p-4">
    <div class="bg-[#fcfaf5] w-full max-w-sm rounded-[32px] shadow-2xl overflow-hidden border border-amber-900/20">
      <div class="farm-pattern p-6 text-center text-white relative border-b-4 border-amber-600">
        <button onclick="toggleLanguage()" class="absolute top-4 right-4 bg-amber-500/20 hover:bg-amber-500/30 backdrop-blur-md px-3 py-1 rounded-full text-xs font-bold text-amber-200 transition flex items-center gap-1.5 border border-amber-400/30">
          <i class="fa-solid fa-language text-amber-300"></i>
          <span class="lang-btn-label">हिन्दी</span>
        </button>
        <div class="w-16 h-16 rounded-2xl bg-gradient-to-tr from-amber-500 to-emerald-600 mx-auto mb-2 flex items-center justify-center shadow-lg border-2 border-amber-300/40">
          <i class="fa-solid fa-wheat-awn text-amber-100 text-3xl"></i>
        </div>
        <h1 class="text-2xl font-black tracking-tight">KisanDirect</h1>
        <p class="text-amber-200/90 text-xs mt-0.5 font-semibold" data-i18n="authSubtitle">धरती का मोल • किसान का हक</p>
      </div>

      <div class="p-6">
        <div class="flex p-1 bg-stone-200/70 rounded-2xl mb-5 text-xs font-bold">
          <button id="tabLogin" onclick="switchAuthTab('login')" class="flex-1 py-2 rounded-xl bg-white shadow-sm text-emerald-900 transition" data-i18n="loginTab">लॉग इन (Log In)</button>
          <button id="tabRegister" onclick="switchAuthTab('register')" class="flex-1 py-2 rounded-xl text-stone-600 hover:text-stone-900 transition" data-i18n="registerTab">नया खाता</button>
        </div>

        <form id="authForm" onsubmit="handleAuthSubmit(event)" class="space-y-3">
          <div id="fieldFullName" class="hidden">
            <label class="block text-[11px] font-bold uppercase text-stone-600 mb-1" data-i18n="fullNameLabel">पूरा नाम</label>
            <input type="text" id="authName" placeholder="e.g. रामेश्वर पटेल" class="w-full px-3.5 py-2.5 text-sm bg-white border border-stone-300 rounded-xl focus:ring-2 focus:ring-amber-500 outline-none">
          </div>

          <div>
            <label class="block text-[11px] font-bold uppercase text-stone-600 mb-1" data-i18n="roleLabel">आपकी भूमिका</label>
            <div class="grid grid-cols-2 gap-2">
              <label class="cursor-pointer border-2 border-stone-200 rounded-xl p-2.5 flex items-center justify-center gap-2 has-[:checked]:border-amber-600 has-[:checked]:bg-amber-50 has-[:checked]:text-amber-900 font-bold text-xs transition">
                <input type="radio" name="authRole" value="Farmer" checked class="hidden">
                <i class="fa-solid fa-tractor text-emerald-700"></i> <span data-i18n="farmerRole">किसान भाई</span>
              </label>
              <label class="cursor-pointer border-2 border-stone-200 rounded-xl p-2.5 flex items-center justify-center gap-2 has-[:checked]:border-amber-600 has-[:checked]:bg-amber-50 has-[:checked]:text-amber-900 font-bold text-xs transition">
                <input type="radio" name="authRole" value="Buyer" class="hidden">
                <i class="fa-solid fa-shop text-amber-700"></i> <span data-i18n="buyerRole">व्यापारी / खरीदार</span>
              </label>
            </div>
          </div>

          <div>
            <label class="block text-[11px] font-bold uppercase text-stone-600 mb-1" data-i18n="phoneLabel">मोबाइल / व्हाट्सएप नंबर</label>
            <input type="tel" id="authPhone" placeholder="10 अंकों का नंबर" pattern="[0-9]{10}" required class="w-full px-3.5 py-2.5 text-sm bg-white border border-stone-300 rounded-xl focus:ring-2 focus:ring-amber-500 outline-none">
          </div>

          <div>
            <label class="block text-[11px] font-bold uppercase text-stone-600 mb-1" data-i18n="pinLabel">सुरक्षा पिन</label>
            <input type="password" id="authPassword" placeholder="••••" minlength="4" required class="w-full px-3.5 py-2.5 text-sm bg-white border border-stone-300 rounded-xl focus:ring-2 focus:ring-amber-500 outline-none">
          </div>

          <button type="submit" id="authSubmitBtn" class="w-full mt-2 py-3 bg-gradient-to-r from-emerald-800 to-green-900 hover:from-emerald-900 hover:to-green-950 text-white font-bold rounded-2xl shadow-lg shadow-emerald-900/30 transition text-sm flex items-center justify-center gap-2">
            <i class="fa-solid fa-seedling text-amber-400"></i> <span data-i18n="loginBtn">मंडी में प्रवेश करें</span>
          </button>
        </form>

        <div class="mt-4 pt-4 border-t border-stone-200 text-center">
          <button onclick="demoLogin()" class="w-full text-xs font-bold py-2.5 bg-amber-100/70 hover:bg-amber-100 text-amber-900 rounded-xl transition flex items-center justify-center gap-2 border border-amber-300">
            <i class="fa-solid fa-bolt text-amber-600"></i> <span data-i18n="demoBtn">1-क्लिक किसान डेमो लॉगिन</span>
          </button>
        </div>
      </div>
    </div>
  </div>

  <!-- ================= MAIN APP ================= -->
  <div id="mainApp" class="hidden">
    
    <!-- Agricultural Header -->
    <header class="bg-[#1b3b22] text-amber-50 border-b-2 border-amber-600/40 sticky top-0 z-40 shadow-md">
      <div class="max-w-4xl mx-auto px-4 py-2.5 flex justify-between items-center">
        <div class="flex items-center space-x-2.5">
          <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-amber-500 to-amber-700 flex items-center justify-center text-white shadow-inner border border-amber-300/40">
            <i class="fa-solid fa-wheat-awn text-stone-950 text-xl"></i>
          </div>
          <div>
            <h1 class="font-black text-lg tracking-tight text-white leading-none flex items-center gap-1.5">
              KisanDirect
              <span class="text-[9px] bg-amber-500 text-stone-950 font-extrabold px-1.5 py-0.2 rounded">ई-मंडी</span>
            </h1>
            <span class="text-[10px] text-amber-200 font-bold tracking-wider" data-i18n="tagline">खेत से मंडी तक सीधा व्यापार</span>
          </div>
        </div>

        <div class="flex items-center gap-2">
          <button onclick="toggleLanguage()" class="bg-emerald-900 hover:bg-emerald-800 border border-amber-500/40 text-amber-200 px-3 py-1 rounded-xl text-xs font-bold flex items-center gap-1.5 transition">
            <i class="fa-solid fa-language text-amber-400"></i>
            <span class="lang-btn-label">English</span>
          </button>
          <button onclick="handleLogout()" class="text-red-300 bg-red-950/50 hover:bg-red-900/70 border border-red-800/40 px-2.5 py-1 rounded-xl text-xs font-bold transition">
            <i class="fa-solid fa-right-from-bracket"></i>
          </button>
        </div>
      </div>
    </header>

    <main class="max-w-4xl mx-auto px-4 py-4 space-y-5">

      <!-- Hero Banner -->
      <div class="farm-pattern rounded-[28px] p-5 sm:p-6 text-white shadow-xl shadow-stone-900/15 border-2 border-amber-600/50 relative overflow-hidden">
        <div class="relative z-10 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
          <div>
            <span class="inline-flex items-center gap-1.5 bg-amber-500/20 backdrop-blur-md px-3 py-1 rounded-full text-[10px] font-bold uppercase tracking-wider text-amber-300 border border-amber-400/30 mb-2">
              <span class="w-2 h-2 rounded-full bg-red-500 animate-ping"></span>
              <span data-i18n="heroPill">लाइव डिजिटल ई-नीलामी चालू है</span>
            </span>
            <h2 class="text-xl sm:text-2xl font-black leading-snug text-white" data-i18n="heroTitle">फसल की खुली ऑनलाइन बोली लगाएं</h2>
            <p class="text-amber-100/90 text-xs sm:text-sm mt-1 max-w-md" data-i18n="heroDesc">खरीदार लाइव बोलियां लगाते हैं। सबसे ऊंची बोली स्वीकार करें और व्हाट्सएप पर सीधा सौदा पक्का करें।</p>
          </div>
          <div class="flex gap-2 w-full sm:w-auto">
            <button onclick="scrollToSection('sectionBidding')" class="whitespace-nowrap bg-gradient-to-r from-amber-400 to-amber-500 hover:from-amber-300 hover:to-amber-400 text-stone-950 font-black px-4 py-2.5 rounded-2xl shadow-lg transition text-xs flex items-center justify-center gap-2 border border-amber-200">
              <i class="fa-solid fa-gavel text-sm"></i> <span data-i18n="viewBidsBtn">बोली केंद्र देखें</span>
            </button>
            <button onclick="openModal()" class="whitespace-nowrap bg-emerald-900/80 hover:bg-emerald-900 text-amber-100 font-bold px-4 py-2.5 rounded-2xl transition text-xs flex items-center justify-center gap-1.5 border border-amber-400/30">
              <i class="fa-solid fa-plus text-xs"></i> <span data-i18n="postProduceBtn">फसल बेचें</span>
            </button>
          </div>
        </div>
      </div>

      <!-- ================= LIVE BIDDING / AUCTION FLOOR SECTION ================= -->
      <section id="sectionBidding" class="bg-[#fffdf9] rounded-[28px] p-5 shadow-sm border-2 border-amber-900/15 space-y-4">
        <div class="flex flex-col sm:flex-row justify-between sm:items-center gap-2">
          <div>
            <div class="flex items-center gap-2">
              <span class="w-8 h-8 rounded-xl bg-red-100 text-red-700 flex items-center justify-center text-sm border border-red-300">
                <i class="fa-solid fa-gavel"></i>
              </span>
              <h2 class="text-base sm:text-lg font-black text-stone-900 flex items-center gap-2" data-i18n="biddingHeader">
                लाइव फसल नीलामी व बोली केंद्र
                <span class="text-[10px] font-extrabold bg-red-600 text-white px-2 py-0.5 rounded-full animate-pulse">LIVE</span>
              </h2>
            </div>
            <p class="text-xs text-stone-600 font-medium" data-i18n="biddingSub">किसान की फसल पर प्रतिस्पर्धी खरीदार वास्तविक समय में उच्चतम बोलियां लगाते हैं</p>
          </div>
          <span class="text-[10px] font-black uppercase tracking-wider bg-amber-100 text-amber-900 px-3 py-1 rounded-full self-start sm:self-auto border border-amber-300">
            पारदर्शी ई-नीलामी
          </span>
        </div>

        <!-- Bidding Cards Grid -->
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4" id="biddingGrid">
          <!-- Rendered via JS -->
        </div>
      </section>

      <!-- ================= MIDDLEMAN SAVINGS LIVE CALCULATOR ================= -->
      <section id="sectionCalculator" class="bg-[#fffdf9] rounded-[28px] p-5 shadow-sm border-2 border-amber-900/15 space-y-4">
        <div class="flex flex-col sm:flex-row justify-between sm:items-center gap-2">
          <div>
            <div class="flex items-center gap-2">
              <span class="w-8 h-8 rounded-xl bg-amber-100 text-amber-900 flex items-center justify-center text-sm border border-amber-300">
                <i class="fa-solid fa-calculator"></i>
              </span>
              <h2 class="text-base sm:text-lg font-black text-stone-900" data-i18n="calcHeader">किसान बचत व शुद्ध मुनाफ़ा कैलकुलेटर</h2>
            </div>
            <p class="text-xs text-stone-600 font-medium" data-i18n="calcSub">देखें कि सीधे बेचने पर बिचौलिया हटने से आपकी जेब में कितने रुपये अतिरिक्त बचते हैं</p>
          </div>
          <span class="text-[10px] font-black uppercase tracking-wider bg-emerald-800 text-amber-200 px-3 py-1 rounded-full self-start sm:self-auto border border-amber-400/40">
            0% दलाली गारंटी
          </span>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-4 bg-[#fbf8f0] p-4 rounded-2xl border border-amber-900/10">
          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1.5" data-i18n="selectCropCalc">फसल का चयन करें</label>
            <select id="calcCropSelect" onchange="updateSavingsCalculator()" class="w-full bg-white border border-amber-900/20 rounded-xl px-3.5 py-2.5 text-xs font-bold text-stone-800 focus:ring-2 focus:ring-amber-500 outline-none">
            </select>
          </div>

          <div>
            <div class="flex justify-between items-center mb-1.5">
              <label class="text-xs font-bold text-stone-700" data-i18n="harvestLotLabel">फसल की मात्रा (क्विंटल में)</label>
              <span id="sliderValueText" class="font-extrabold text-sm text-emerald-900 bg-emerald-100 px-2.5 py-0.5 rounded-lg border border-emerald-300">100 Quintals</span>
            </div>
            <input type="range" id="calcQtySlider" min="5" max="500" step="5" value="100" oninput="updateSavingsCalculator()" class="w-full bg-stone-300 rounded-lg h-2 outline-none">
            
            <div class="flex gap-2 mt-2">
              <button onclick="setSliderQty(25)" class="text-[10px] font-bold bg-white border border-stone-300 hover:bg-amber-50 text-stone-700 px-2.5 py-1 rounded-lg">25 qtl</button>
              <button onclick="setSliderQty(50)" class="text-[10px] font-bold bg-white border border-stone-300 hover:bg-amber-50 text-stone-700 px-2.5 py-1 rounded-lg">50 qtl</button>
              <button onclick="setSliderQty(100)" class="text-[10px] font-bold bg-amber-500 text-stone-950 px-2.5 py-1 rounded-lg">100 qtl</button>
              <button onclick="setSliderQty(200)" class="text-[10px] font-bold bg-white border border-stone-300 hover:bg-amber-50 text-stone-700 px-2.5 py-1 rounded-lg">200 qtl</button>
            </div>
          </div>
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3.5">
          <div class="bg-red-50/70 border border-red-200 rounded-2xl p-4 flex flex-col justify-between">
            <div>
              <div class="flex justify-between items-center">
                <span class="text-[11px] font-bold text-red-900 flex items-center gap-1.5">
                  <i class="fa-solid fa-circle-xmark text-red-600"></i> <span data-i18n="middlemanPayout">स्थानीय बिचौलिया भुगतान</span>
                </span>
                <span id="calcMiddlemanRate" class="text-[10px] font-bold text-red-700">@ ₹2,200/qtl</span>
              </div>
              <h3 id="calcMiddlemanTotal" class="text-xl sm:text-2xl font-black text-red-800 mt-2 line-through">₹2,20,000</h3>
            </div>
            <p class="text-[10px] text-red-700/80 mt-2 font-semibold" data-i18n="middlemanLossDesc">कमीशन और तुलाई के नाम पर किसान को भारी नुकसान होता है।</p>
          </div>

          <div class="bg-gradient-to-br from-emerald-800 to-green-900 text-white rounded-2xl p-4 flex flex-col justify-between shadow-md border border-amber-400/30">
            <div>
              <div class="flex justify-between items-center">
                <span class="text-[11px] font-bold text-amber-200 flex items-center gap-1.5">
                  <i class="fa-solid fa-circle-check text-amber-400"></i> <span data-i18n="kisanDirectPayout">किसानडायरेक्ट से सीधा भुगतान</span>
                </span>
                <span id="calcFairRate" class="text-[10px] font-bold text-emerald-200">@ ₹2,650/qtl</span>
              </div>
              <h3 id="calcDirectTotal" class="text-xl sm:text-2xl font-black text-white mt-2">₹2,65,000</h3>
            </div>
            <p class="text-[10px] text-emerald-100 mt-2 font-semibold" data-i18n="kisanDirectDesc">बिना किसी दलाली के पूरी फसल का सही भाव सीधे आपके खाते में।</p>
          </div>
        </div>

        <div class="bg-amber-100/60 border-2 border-amber-400/60 rounded-2xl p-4 flex flex-col sm:flex-row justify-between items-center gap-3">
          <div class="flex items-center gap-3 text-center sm:text-left">
            <div class="w-12 h-12 rounded-2xl bg-amber-500 text-stone-950 flex items-center justify-center text-xl shrink-0 shadow-md">
              <i class="fa-solid fa-sack-dollar"></i>
            </div>
            <div>
              <span class="text-[11px] font-bold text-stone-600 uppercase tracking-wider" data-i18n="youKeepExtra">आपकी अतिरिक्त शुद्ध बचत</span>
              <h4 id="calcExtraProfit" class="text-lg sm:text-xl font-black text-emerald-950 leading-tight">+₹45,000 Extra in Your Pocket</h4>
            </div>
          </div>
          <button onclick="quickSellCalculatedCrop()" class="w-full sm:w-auto bg-amber-600 hover:bg-amber-700 text-white font-black px-5 py-2.5 rounded-xl text-xs transition shadow-sm flex items-center justify-center gap-1.5">
            <i class="fa-solid fa-hand-holding-dollar"></i> <span data-i18n="lockSellDirect">सीधे भाव पर बेचें</span>
          </button>
        </div>
      </section>

      <!-- Section: Real-Time Mandi Prices -->
      <section id="sectionPrices" class="bg-[#fffdf9] rounded-[28px] p-5 shadow-sm border-2 border-amber-900/15 space-y-4">
        <div class="flex flex-col sm:flex-row justify-between sm:items-center gap-3">
          <div>
            <h2 class="text-base sm:text-lg font-black text-stone-900 flex items-center gap-2">
              <i class="fa-solid fa-scale-balanced text-amber-600"></i> <span data-i18n="ratesHeader">दैनिक मंडी भाव व उचित मूल्य</span>
            </h2>
            <p class="text-xs text-stone-600 font-medium" data-i18n="ratesSub">प्रति क्विंटल (100 कि.ग्रा.) आज के ताज़ा मॉडल भाव</p>
          </div>
          <div class="relative">
            <i class="fa-solid fa-magnifying-glass absolute left-3.5 top-1/2 -translate-y-1/2 text-stone-400 text-xs"></i>
            <input type="text" id="searchInput" onkeyup="filterCrops()" placeholder="फसल का नाम खोजें..." 
                   class="w-full sm:w-60 pl-9 pr-3.5 py-2 text-xs bg-[#fbf8f0] border border-stone-300 rounded-xl focus:ring-2 focus:ring-amber-500 outline-none font-bold transition">
          </div>
        </div>

        <div class="flex gap-2 overflow-x-auto no-scrollbar py-1 text-xs font-bold" id="categoryFilters">
        </div>

        <div class="grid grid-cols-1 sm:grid-cols-2 gap-3" id="cropPricesContainer">
        </div>
      </section>

    </main>

    <!-- ================= BOTTOM NAVIGATION ================= -->
    <nav class="fixed bottom-0 inset-x-0 bg-[#1b3b22]/95 backdrop-blur-lg border-t-2 border-amber-600/40 px-4 py-2 z-40 flex justify-around items-center max-w-lg mx-auto md:rounded-t-3xl shadow-2xl">
      <button onclick="scrollToSection('sectionBidding')" class="flex flex-col items-center gap-1 text-amber-300 font-extrabold text-[10px] transition">
        <i class="fa-solid fa-gavel text-lg"></i>
        <span data-i18n="navBidding">बोली केंद्र</span>
      </button>
      <button onclick="scrollToSection('sectionCalculator')" class="flex flex-col items-center gap-1 text-amber-200/70 hover:text-amber-300 font-bold text-[10px] transition">
        <i class="fa-solid fa-calculator text-lg"></i>
        <span data-i18n="navCalc">बचत कैलकुलेटर</span>
      </button>
      <button onclick="openModal()" class="flex flex-col items-center -mt-5 bg-gradient-to-tr from-amber-400 to-amber-500 text-stone-950 w-12 h-12 rounded-full shadow-lg shadow-amber-500/40 justify-center active:scale-95 transition border-2 border-white">
        <i class="fa-solid fa-plus text-lg"></i>
      </button>
      <button onclick="scrollToSection('sectionPrices')" class="flex flex-col items-center gap-1 text-amber-200/70 hover:text-amber-300 font-bold text-[10px] transition">
        <i class="fa-solid fa-chart-line text-lg"></i>
        <span data-i18n="navRates">मंडी भाव</span>
      </button>
      <button onclick="demoLogin()" class="flex flex-col items-center gap-1 text-amber-200/70 hover:text-amber-300 font-bold text-[10px] transition">
        <i class="fa-regular fa-circle-user text-lg"></i>
        <span data-i18n="navProfile">किसान खाता</span>
      </button>
    </nav>

    <!-- ================= BIDDING MODAL (PLACE OFFER) ================= -->
    <div id="bidModal" class="hidden fixed inset-0 bg-stone-950/70 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-[#fcfaf5] rounded-[32px] w-full max-w-sm p-6 shadow-2xl space-y-4 border-2 border-amber-800/20">
        <div class="flex justify-between items-center border-b border-stone-200 pb-3">
          <div>
            <h3 class="font-black text-stone-900 text-lg flex items-center gap-1.5">
              <i class="fa-solid fa-gavel text-amber-600"></i> <span data-i18n="placeBidHeader">अपनी बोली लगाएं</span>
            </h3>
            <p id="bidModalSubtitle" class="text-xs text-stone-600">Sharbati Wheat • 60 Quintals</p>
          </div>
          <button onclick="closeBidModal()" class="text-stone-400 hover:text-stone-700 w-8 h-8 rounded-full bg-stone-200 flex items-center justify-center text-sm font-bold">&times;</button>
        </div>

        <form id="bidForm" onsubmit="handlePlaceBid(event)" class="space-y-3">
          <div class="p-3 bg-amber-50 rounded-2xl border border-amber-200 flex justify-between items-center">
            <span class="text-xs text-stone-600 font-bold" data-i18n="currentTopBid">वर्तमान उच्चतम बोली:</span>
            <span id="bidModalCurrentPrice" class="text-sm font-black text-emerald-900">₹2,720/qtl</span>
          </div>

          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="yourBidAmount">आपकी नई बोली प्रति क्विंटल (₹)</label>
            <input type="number" id="bidInputAmount" required class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2.5 text-base font-black text-stone-900 focus:ring-2 focus:ring-amber-500 outline-none">
            <p class="text-[10px] text-stone-500 mt-1" data-i18n="bidNotice">बोली वर्तमान उच्चतम बोली से अधिक होनी चाहिए।</p>
          </div>

          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="buyerNameLabel">खरीदार / मिल का नाम</label>
            <input type="text" id="bidInputBuyer" placeholder="e.g. Shyam Agro Mills" required class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2 text-xs font-bold focus:ring-2 focus:ring-amber-500 outline-none">
          </div>

          <button type="submit" class="w-full bg-amber-600 hover:bg-amber-700 active:scale-95 text-white font-black py-3 rounded-2xl shadow-lg transition text-xs flex items-center justify-center gap-2">
            <i class="fa-solid fa-check"></i> <span data-i18n="confirmBidBtn">बोली पुष्टि करें</span>
          </button>
        </form>
      </div>
    </div>

    <!-- ================= POST HARVEST MODAL ================= -->
    <div id="harvestModal" class="hidden fixed inset-0 bg-stone-950/70 backdrop-blur-sm z-50 flex items-center justify-center p-4">
      <div class="bg-[#fcfaf5] rounded-[32px] w-full max-w-md p-6 shadow-2xl space-y-4 max-h-[90vh] overflow-y-auto no-scrollbar border-2 border-amber-800/20">
        <div class="flex justify-between items-center border-b border-stone-200 pb-3">
          <div>
            <h3 class="font-black text-stone-900 text-lg flex items-center gap-1.5" data-i18n="modalTitle">
              <i class="fa-solid fa-wheat-awn text-amber-600"></i> अपनी फसल दर्ज करें
            </h3>
            <p class="text-xs text-stone-600" data-i18n="modalSubtitle">सीधे खरीदारों को 0% कमीशन पर बेचें</p>
          </div>
          <button onclick="closeModal()" class="text-stone-400 hover:text-stone-700 w-8 h-8 rounded-full bg-stone-200 flex items-center justify-center text-sm font-bold">&times;</button>
        </div>

        <form id="harvestForm" onsubmit="handleFormSubmit(event)" class="space-y-3.5">
          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="photoLabel">फसल की फोटो</label>
            <div class="border-2 border-dashed border-amber-900/30 rounded-2xl p-4 text-center hover:border-amber-600 transition relative bg-[#fbf8f0] overflow-hidden">
              <input type="file" id="cropImageInput" accept="image/*" onchange="previewCropImage(event)" class="absolute inset-0 w-full h-full opacity-0 cursor-pointer z-10">
              <div id="uploadPlaceholder" class="space-y-1.5 py-2">
                <div class="w-10 h-10 rounded-full bg-amber-100 text-amber-800 mx-auto flex items-center justify-center text-lg">
                  <i class="fa-solid fa-camera"></i>
                </div>
                <p class="text-xs text-stone-800 font-bold" data-i18n="photoPrompt">खेत से सीधी फोटो खींचें या चुनें</p>
                <p class="text-[10px] text-stone-500" data-i18n="photoTip">साफ फोटो देखकर खरीदार तुरंत संपर्क करते हैं</p>
              </div>
              <div id="imagePreviewContainer" class="hidden relative">
                <img id="imagePreview" src="" alt="Crop Quality Preview" class="w-full h-40 object-cover rounded-xl border border-stone-300">
                <button type="button" onclick="removeImage(event)" class="absolute top-2 right-2 bg-red-600 text-white rounded-full w-7 h-7 flex items-center justify-center text-xs shadow-md z-20">
                  <i class="fa-solid fa-times"></i>
                </button>
              </div>
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="selectCropLabel">फसल चुनें</label>
            <select id="modalCropName" onchange="onModalCropSelect(this.value)" class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2.5 text-xs font-bold focus:ring-2 focus:ring-amber-500 outline-none" required>
            </select>
          </div>

          <div class="grid grid-cols-2 gap-3">
            <div>
              <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="qtyLabel">मात्रा (क्विंटल)</label>
              <input type="number" id="modalQuantity" placeholder="e.g. 60" required class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2 text-xs font-bold focus:ring-2 focus:ring-amber-500 outline-none">
            </div>
            <div>
              <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="priceLabel">आधार भाव / क्विंटल (₹)</label>
              <input type="number" id="modalPrice" placeholder="e.g. 2650" required class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2 text-xs font-bold focus:ring-2 focus:ring-amber-500 outline-none">
            </div>
          </div>

          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="farmerNameLabel">किसान का नाम</label>
            <input type="text" id="modalFarmer" placeholder="e.g. रामेश्वर पटेल" required class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2 text-xs font-bold focus:ring-2 focus:ring-amber-500 outline-none">
          </div>

          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="locationLabel">खेत का पता / मंडी</label>
            <input type="text" id="modalLocation" placeholder="गाँव, तहसील, जिला" required class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2 text-xs font-bold focus:ring-2 focus:ring-amber-500 outline-none">
          </div>

          <div>
            <label class="block text-xs font-bold text-stone-700 mb-1" data-i18n="whatsappLabel">व्हाट्सएप नंबर (कंट्री कोड सहित)</label>
            <input type="tel" id="modalWhatsApp" placeholder="e.g. 919876543210" pattern="[0-9]{10,14}" required class="w-full bg-white border border-stone-300 rounded-xl px-3.5 py-2 text-xs font-bold focus:ring-2 focus:ring-amber-500 outline-none">
          </div>

          <div class="pt-2">
            <button type="submit" class="w-full bg-gradient-to-r from-emerald-800 to-green-900 hover:from-emerald-900 hover:to-green-950 text-white font-black py-3 rounded-2xl shadow-lg transition text-xs flex items-center justify-center gap-2">
              <i class="fa-solid fa-paper-plane text-amber-400"></i> <span data-i18n="publishBtn">फसल लाइव नीलामी में दर्ज करें</span>
            </button>
          </div>
        </form>
      </div>
    </div>

  </div>

  <script>
    // ================= FULL BILINGUAL TRANSLATION DICTIONARY =================
    let currentLang = localStorage.getItem('kisandirect_lang') || 'hi';

    const translations = {
      hi: {
        langBtn: "English",
        authSubtitle: "धरती का मोल • किसान का हक",
        loginTab: "लॉग इन",
        registerTab: "नया खाता",
        fullNameLabel: "पूरा नाम",
        roleLabel: "आपकी भूमिका",
        farmerRole: "किसान भाई",
        buyerRole: "व्यापारी / खरीदार",
        phoneLabel: "मोबाइल / व्हाट्सएप नंबर",
        pinLabel: "सुरक्षा पिन",
        loginBtn: "मंडी में प्रवेश करें",
        demoBtn: "1-क्लिक किसान डेमो लॉगिन",
        tagline: "खेत से मंडी तक सीधा व्यापार",
        exitBtn: "बाहर निकलें",
        heroPill: "लाइव डिजिटल ई-नीलामी चालू है",
        heroTitle: "फसल की खुली ऑनलाइन बोली लगाएं",
        heroDesc: "खरीदार लाइव बोलियां लगाते हैं। सबसे ऊंची बोली स्वीकार करें और व्हाट्सएप पर सीधा सौदा पक्का करें।",
        viewBidsBtn: "बोली केंद्र देखें",
        postProduceBtn: "फसल बेचें",
        biddingHeader: "लाइव फसल नीलामी व बोली केंद्र",
        biddingSub: "किसान की फसल पर प्रतिस्पर्धी खरीदार वास्तविक समय में उच्चतम बोलियां लगाते हैं",
        calcHeader: "किसान बचत व शुद्ध मुनाफ़ा कैलकुलेटर",
        calcSub: "देखें कि सीधे बेचने पर बिचौलिया हटने से आपकी जेब में कितने रुपये अतिरिक्त बचते हैं",
        selectCropCalc: "फसल का चयन करें",
        harvestLotLabel: "फसल की मात्रा (क्विंटल में)",
        middlemanPayout: "स्थानीय बिचौलिया भुगतान",
        middlemanLossDesc: "कमीशन और तुलाई के नाम पर किसान को भारी नुकसान होता है।",
        kisanDirectPayout: "किसानडायरेक्ट से सीधा भुगतान",
        kisanDirectDesc: "बिना किसी दलाली के पूरी फसल का सही भाव सीधे आपके खाते में।",
        youKeepExtra: "आपकी अतिरिक्त शुद्ध बचत",
        lockSellDirect: "सीधे भाव पर बेचें",
        ratesHeader: "दैनिक मंडी भाव व उचित मूल्य",
        ratesSub: "प्रति क्विंटल (100 कि.ग्रा.) आज के ताज़ा मॉडल भाव",
        searchPlaceholder: "फसल का नाम खोजें...",
        navBidding: "बोली केंद्र",
        navCalc: "बचत कैलकुलेटर",
        navRates: "मंडी भाव",
        navProfile: "किसान खाता",
        modalTitle: "अपनी फसल दर्ज करें",
        modalSubtitle: "सीधे खरीदारों को 0% कमीशन पर बेचें",
        photoLabel: "फसल की फोटो",
        photoPrompt: "खेत से सीधी फोटो खींचें या चुनें",
        photoTip: "साफ फोटो देखकर खरीदार तुरंत संपर्क करते हैं",
        selectCropLabel: "फसल चुनें",
        qtyLabel: "मात्रा (क्विंटल)",
        priceLabel: "आधार भाव / क्विंटल (₹)",
        farmerNameLabel: "किसान का नाम",
        locationLabel: "खेत का पता / मंडी",
        whatsappLabel: "व्हाट्सएप नंबर (कंट्री कोड सहित)",
        publishBtn: "फसल लाइव नीलामी में दर्ज करें",
        sellDirectBtn: "सीधे बेचें",
        moreProfit: "अतिरिक्त लाभ",
        allCategories: "सभी फसलें",
        catCereals: "अनाज (गेहूँ/धान)",
        catPulses: "दालें (चना/मूँग)",
        catOilseeds: "तिलहन (सोयाबीन/सरसों)",
        endsIn: "समय शेष",
        currentTopBid: "वर्तमान उच्चतम बोली",
        basePrice: "आधार मूल्य",
        placeBidBtn: "बोली लगाएं",
        acceptBidBtn: "बोली स्वीकारें",
        bidsPlaced: "बोलियां लगीं",
        placeBidHeader: "अपनी बोली लगाएं",
        yourBidAmount: "आपकी नई बोली प्रति क्विंटल (₹)",
        buyerNameLabel: "खरीदार / मिल का नाम",
        confirmBidBtn: "बोली पुष्टि करें",
        bidNotice: "बोली वर्तमान उच्चतम बोली से अधिक होनी चाहिए।",
        waAcceptMessage: (farmer, buyer, crop, bid) => `नमस्ते ${buyer} जी! मैं किसान ${farmer}। मुझे आपकी ₹${bid}/क्विंटल की बोली स्वीकार है ${crop} के लिए। आइए सौदे की डिलीवरी फाइनल करें।`
      },
      en: {
        langBtn: "हिन्दी",
        authSubtitle: "True Crop Value • Farmer's Right",
        loginTab: "Log In",
        registerTab: "Sign Up",
        fullNameLabel: "Full Name",
        roleLabel: "Your Role",
        farmerRole: "Farmer",
        buyerRole: "Direct Buyer",
        phoneLabel: "Mobile / WhatsApp Number",
        pinLabel: "Security PIN",
        loginBtn: "Enter Kisan Marketplace",
        demoBtn: "1-Click Farmer Demo Login",
        tagline: "DIRECT FROM FARM TO CONSUMER",
        exitBtn: "Exit",
        heroPill: "Live Digital E-Auction Active",
        heroTitle: "Transparent Open Bidding Floor",
        heroDesc: "Buyers place competitive bids. Accept top offer and close deal with 1-tap WhatsApp.",
        viewBidsBtn: "View Bidding Floor",
        postProduceBtn: "List My Crop",
        biddingHeader: "Live Crop Bidding & Auctions",
        biddingSub: "Verified retail and mill buyers place real-time bids on farm harvests",
        calcHeader: "Middleman Savings Live Calculator",
        calcSub: "See exactly how much extra money stays in your pocket when eliminating brokers",
        selectCropCalc: "Select Crop",
        harvestLotLabel: "Harvest Quantity (Quintals / 100kg)",
        middlemanPayout: "Middleman / Dealer Payout",
        middlemanLossDesc: "Brokers slash prices by 15-25% claiming grading cuts and commissions.",
        kisanDirectPayout: "KisanDirect Payout",
        kisanDirectDesc: "Full fair market value transferred directly without any cuts.",
        youKeepExtra: "Your Extra Earnings",
        lockSellDirect: "Lock & Sell Direct",
        ratesHeader: "Daily Mandi vs. Direct Fair Rates",
        ratesSub: "Modal rate per Quintal (100 kg) updated today",
        searchPlaceholder: "Search crop or variety...",
        navBidding: "Bidding Floor",
        navCalc: "Savings Calc",
        navRates: "Live Rates",
        navProfile: "Profile",
        modalTitle: "List Your Produce",
        modalSubtitle: "Sell directly to retail buyers with 0% brokerage",
        photoLabel: "Crop Quality Photo",
        photoPrompt: "Snap photo or choose from gallery",
        photoTip: "Clear photos attract immediate buyer inquiries",
        selectCropLabel: "Select Crop",
        qtyLabel: "Quantity (Quintals)",
        priceLabel: "Base Price / Quintal (₹)",
        farmerNameLabel: "Farmer Name",
        locationLabel: "Farm Location / Mandi",
        whatsappLabel: "WhatsApp Number (with country code)",
        publishBtn: "List Produce in Live Auction",
        sellDirectBtn: "Sell Direct",
        moreProfit: "More",
        allCategories: "All Crops",
        catCereals: "Grains & Cereals",
        catPulses: "Pulses (Dal)",
        catOilseeds: "Oilseeds",
        endsIn: "Time Left",
        currentTopBid: "Highest Bid",
        basePrice: "Base Price",
        placeBidBtn: "Place Bid",
        acceptBidBtn: "Accept Bid",
        bidsPlaced: "Bids placed",
        placeBidHeader: "Place Your Bid",
        yourBidAmount: "Your Bid per Quintal (₹)",
        buyerNameLabel: "Buyer / Mill Name",
        confirmBidBtn: "Confirm Bid",
        bidNotice: "Offer must be higher than current top bid.",
        waAcceptMessage: (farmer, buyer, crop, bid) => `Namaste ${buyer} ji! I am farmer ${farmer}. I accept your top bid of ₹${bid}/Quintal for ${crop}. Let us confirm delivery details.`
      }
    };

    // ================= AUCTION / BIDDING DATABASE =================
    let biddingListings = [
      {
        id: 201,
        cropEn: "Wheat (Sharbati Gold)",
        cropHi: "गेहूँ (शरबती गोल्ड)",
        farmer: "रामेश्वर पटेल",
        location: "सीहोर, मध्य प्रदेश",
        qty: 60,
        basePrice: 2600,
        currentBid: 2780,
        leadingBuyer: "Shree Ganesh Agro Mills",
        leadingBuyerPhone: "919876501122",
        bidCount: 8,
        timeLeft: "01h 45m",
        image: "https://images.unsplash.com/photo-1574323347407-f5e1ad6d020b?w=600&auto=format&fit=crop&q=80"
      },
      {
        id: 202,
        cropEn: "Basmati Paddy (1121 Export)",
        cropHi: "बासमती धान (1121 एक्सपोर्ट)",
        farmer: "हरप्रीत सिंह",
        location: "करनाल, हरियाणा",
        qty: 120,
        basePrice: 4700,
        currentBid: 5120,
        leadingBuyer: "Kohinoor Grain Traders",
        leadingBuyerPhone: "919811223344",
        bidCount: 14,
        timeLeft: "03h 10m",
        image: "https://images.unsplash.com/photo-1586201375761-83865001e31c?w=600&auto=format&fit=crop&q=80"
      },
      {
        id: 203,
        cropEn: "Red Onion (Nashik Super)",
        cropHi: "लाल प्याज (नासिक सुपर)",
        farmer: "बापूराव जाधव",
        location: "लासलगांव, महाराष्ट्र",
        qty: 85,
        basePrice: 1800,
        currentBid: 2040,
        leadingBuyer: "Mumbai Fresh Wholesalers",
        leadingBuyerPhone: "919822334455",
        bidCount: 9,
        timeLeft: "00h 35m",
        image: "https://images.unsplash.com/photo-1618512496248-a07fe83aa8cb?w=600&auto=format&fit=crop&q=80"
      }
    ];

    // ================= COMMODITY DATABASE =================
    const allCrops = [
      { nameEn: "Wheat (Sharbati)", nameHi: "गेहूँ (शरबती)", category: "Cereals", mandi: 2750, middleman: 2200, fair: 2650, trend: "+1.8%", image: "https://images.unsplash.com/photo-1574323347407-f5e1ad6d020b?w=400&auto=format&fit=crop&q=80" },
      { nameEn: "Basmati Paddy (1121)", nameHi: "बासमती धान (1121)", category: "Cereals", mandi: 4900, middleman: 4100, fair: 4750, trend: "+2.5%", image: "https://images.unsplash.com/photo-1586201375761-83865001e31c?w=400&auto=format&fit=crop&q=80" },
      { nameEn: "Soybean (Yellow)", nameHi: "सोयाबीन (पीला)", category: "Oilseeds", mandi: 5600, middleman: 4600, fair: 5400, trend: "+1.2%", image: "https://images.unsplash.com/photo-1599940824399-b87987ceb72a?w=400&auto=format&fit=crop&q=80" },
      { nameEn: "Mustard Seed (Sarson)", nameHi: "सरसों (राई)", category: "Oilseeds", mandi: 8400, middleman: 6900, fair: 8150, trend: "+0.6%", image: "https://images.unsplash.com/photo-1615485500704-8e990f9900f7?w=400&auto=format&fit=crop&q=80" },
      { nameEn: "Gram / Chana (Desi)", nameHi: "चना (देसी)", category: "Pulses", mandi: 6500, middleman: 5300, fair: 6250, trend: "+1.9%", image: "https://images.unsplash.com/photo-1515543237350-b3eea1ec8082?w=400&auto=format&fit=crop&q=80" },
      { nameEn: "Red Onion (Nashik)", nameHi: "लाल प्याज (नासिक)", category: "Vegetables & Spices", mandi: 2100, middleman: 1400, fair: 1950, trend: "+4.8%", image: "https://images.unsplash.com/photo-1618512496248-a07fe83aa8cb?w=400&auto=format&fit=crop&q=80" }
    ];

    let activeCategory = 'All';
    let currentUser = null;
    let authMode = 'login';
    let uploadedImageBase64 = null;
    let activeBiddingTargetId = null;

    // ================= RENDER BIDDING FLOOR =================
    function renderBiddingFloor() {
      const container = document.getElementById('biddingGrid');
      const t = translations[currentLang];

      container.innerHTML = biddingListings.map(lot => {
        const cropTitle = (currentLang === 'hi') ? lot.cropHi : lot.cropEn;
        const profitOverBase = lot.currentBid - lot.basePrice;
        const acceptWaUrl = `https://wa.me/${lot.leadingBuyerPhone}?text=${encodeURIComponent(t.waAcceptMessage(lot.farmer, lot.leadingBuyer, cropTitle, lot.currentBid))}`;

        return `
        <div class="bg-gradient-to-br from-[#fbf8f0] to-amber-50/50 border-2 border-amber-900/15 rounded-3xl overflow-hidden shadow-xs hover:shadow-md transition flex flex-col justify-between">
          <div class="relative h-44 w-full overflow-hidden bg-stone-200">
            <img src="${lot.image}" alt="${cropTitle}" class="w-full h-full object-cover">
            <div class="absolute inset-0 bg-gradient-to-t from-stone-950/80 via-transparent to-transparent"></div>
            
            <!-- Live Time badge -->
            <span class="absolute top-3 left-3 bg-red-600 text-white text-[10px] font-black px-2.5 py-1 rounded-xl shadow flex items-center gap-1.5">
              <span class="w-1.5 h-1.5 rounded-full bg-white animate-ping"></span>
              ${t.endsIn}: ${lot.timeLeft}
            </span>

            <span class="absolute top-3 right-3 bg-amber-500 text-stone-950 text-[10px] font-black px-2.5 py-1 rounded-xl border border-amber-200 shadow">
              ${lot.bidCount} ${t.bidsPlaced}
            </span>

            <div class="absolute bottom-3 left-3 right-3 flex justify-between items-end text-white">
              <div>
                <span class="text-[10px] text-amber-200 font-bold block">${t.basePrice}: ₹${lot.basePrice}/qtl</span>
                <span class="text-xl font-black text-amber-400">₹${lot.currentBid}<span class="text-xs font-normal text-white">/qtl</span></span>
              </div>
              <span class="text-xs font-bold bg-white/20 backdrop-blur-md px-2.5 py-1 rounded-xl">${lot.qty} Qtl</span>
            </div>
          </div>

          <div class="p-4 space-y-3">
            <div>
              <h3 class="font-black text-stone-900 text-base leading-tight">${cropTitle}</h3>
              <p class="text-xs text-stone-600 font-semibold mt-1">
                <i class="fa-solid fa-location-dot text-amber-600 mr-1"></i> ${lot.location} • <strong class="text-stone-800">${lot.farmer}</strong>
              </p>
            </div>

            <!-- Top Bidder Indicator -->
            <div class="bg-white p-2.5 rounded-xl border border-amber-900/10 flex justify-between items-center text-xs">
              <div>
                <span class="text-[10px] text-stone-500 font-bold block" data-i18n="currentTopBid">वर्तमान उच्चतम बोली:</span>
                <span class="font-extrabold text-stone-800">${lot.leadingBuyer}</span>
              </div>
              <span class="text-[10px] font-black text-emerald-800 bg-emerald-100 px-2 py-0.5 rounded-lg">
                +₹${profitOverBase}
              </span>
            </div>

            <div class="grid grid-cols-2 gap-2 pt-1">
              <button onclick="openBidModal(${lot.id})" class="py-2.5 px-3 bg-amber-600 hover:bg-amber-700 active:scale-95 text-white text-xs font-black rounded-xl shadow-xs transition flex items-center justify-center gap-1.5">
                <i class="fa-solid fa-gavel"></i> <span>${t.placeBidBtn}</span>
              </button>
              <a href="${acceptWaUrl}" target="_blank" rel="noopener noreferrer" class="py-2.5 px-3 bg-emerald-800 hover:bg-emerald-900 active:scale-95 text-white text-xs font-black rounded-xl shadow-xs transition flex items-center justify-center gap-1.5">
                <i class="fa-brands fa-whatsapp text-green-300"></i> <span>${t.acceptBidBtn}</span>
              </a>
            </div>
          </div>
        </div>
        `;
      }).join('');
    }

    function openBidModal(lotId) {
      activeBiddingTargetId = lotId;
      const lot = biddingListings.find(l => l.id === lotId);
      if (!lot) return;

      const cropTitle = (currentLang === 'hi') ? lot.cropHi : lot.cropEn;
      document.getElementById('bidModalSubtitle').innerText = `${cropTitle} • ${lot.qty} Quintals`;
      document.getElementById('bidModalCurrentPrice').innerText = `₹${lot.currentBid}/qtl`;
      document.getElementById('bidInputAmount').value = lot.currentBid + 50;
      document.getElementById('bidInputAmount').min = lot.currentBid + 1;
      document.getElementById('bidModal').classList.remove('hidden');
    }

    function closeBidModal() {
      document.getElementById('bidModal').classList.add('hidden');
      document.getElementById('bidForm').reset();
    }

    function handlePlaceBid(e) {
      e.preventDefault();
      const newBidAmount = parseInt(document.getElementById('bidInputAmount').value);
      const buyerName = document.getElementById('bidInputBuyer').value;
      const lot = biddingListings.find(l => l.id === activeBiddingTargetId);

      if (lot && newBidAmount > lot.currentBid) {
        lot.currentBid = newBidAmount;
        lot.leadingBuyer = buyerName;
        lot.bidCount += 1;
        renderBiddingFloor();
        closeBidModal();
      } else {
        alert(translations[currentLang].bidNotice);
      }
    }

    // ================= SAVINGS CALCULATOR =================
    function populateCalculatorCrops() {
      const select = document.getElementById('calcCropSelect');
      select.innerHTML = allCrops.map(c => {
        const name = (currentLang === 'hi') ? c.nameHi : c.nameEn;
        return `<option value="${c.nameEn}">${name}</option>`;
      }).join('');
    }

    function setSliderQty(val) {
      document.getElementById('calcQtySlider').value = val;
      updateSavingsCalculator();
    }

    function updateSavingsCalculator() {
      const selectedCropEn = document.getElementById('calcCropSelect').value;
      const crop = allCrops.find(c => c.nameEn === selectedCropEn) || allCrops[0];
      const qty = parseInt(document.getElementById('calcQtySlider').value);

      document.getElementById('sliderValueText').innerText = `${qty} Quintals`;

      const middlemanTotal = crop.middleman * qty;
      const directTotal = crop.fair * qty;
      const extraProfit = directTotal - middlemanTotal;

      document.getElementById('calcMiddlemanRate').innerText = `@ ₹${crop.middleman.toLocaleString('en-IN')}/qtl`;
      document.getElementById('calcMiddlemanTotal').innerText = `₹${middlemanTotal.toLocaleString('en-IN')}`;

      document.getElementById('calcFairRate').innerText = `@ ₹${crop.fair.toLocaleString('en-IN')}/qtl`;
      document.getElementById('calcDirectTotal').innerText = `₹${directTotal.toLocaleString('en-IN')}`;

      const profitText = (currentLang === 'hi') 
        ? `+₹${extraProfit.toLocaleString('en-IN')} आपकी जेब में शुद्ध बचत!` 
        : `+₹${extraProfit.toLocaleString('en-IN')} Extra in Your Pocket!`;
      
      document.getElementById('calcExtraProfit').innerText = profitText;
    }

    function quickSellCalculatedCrop() {
      const selectedCropEn = document.getElementById('calcCropSelect').value;
      const crop = allCrops.find(c => c.nameEn === selectedCropEn) || allCrops[0];
      const qty = document.getElementById('calcQtySlider').value;

      document.getElementById('modalCropName').value = crop.nameEn;
      document.getElementById('modalQuantity').value = qty;
      document.getElementById('modalPrice').value = crop.fair;
      onModalCropSelect(crop.nameEn);
      openModal();
    }

    // ================= LANGUAGE TOGGLE =================
    function toggleLanguage() {
      currentLang = (currentLang === 'en') ? 'hi' : 'en';
      localStorage.setItem('kisandirect_lang', currentLang);
      applyTranslations();
    }

    function applyTranslations() {
      const t = translations[currentLang];

      document.querySelectorAll('[data-i18n]').forEach(el => {
        const key = el.getAttribute('data-i18n');
        if (t[key]) el.innerText = t[key];
      });

      document.querySelectorAll('.lang-btn-label').forEach(el => {
        el.innerText = t.langBtn;
      });

      const searchInput = document.getElementById('searchInput');
      if (searchInput) searchInput.placeholder = t.searchPlaceholder;

      renderCategoryTabs();
      populateCalculatorCrops();
      populateModalCropOptions();
      updateSavingsCalculator();
      filterCrops();
      renderBiddingFloor();
    }

    function renderCategoryTabs() {
      const t = translations[currentLang];
      const categories = [
        { id: 'All', label: `${t.allCategories} (${allCrops.length})` },
        { id: 'Cereals', label: t.catCereals },
        { id: 'Pulses', label: t.catPulses },
        { id: 'Oilseeds', label: t.catOilseeds }
      ];

      const container = document.getElementById('categoryFilters');
      container.innerHTML = categories.map(cat => `
        <button onclick="selectCategory('${cat.id}')" 
                class="cat-pill ${activeCategory === cat.id ? 'bg-amber-600 text-white shadow-sm' : 'bg-[#fbf8f0] border border-stone-300 text-stone-700 hover:bg-amber-50'} px-3.5 py-1.5 rounded-xl whitespace-nowrap transition font-bold">
          ${cat.label}
        </button>
      `).join('');
    }

    function getCropName(crop) {
      return (currentLang === 'hi') ? crop.nameHi : crop.nameEn;
    }

    function renderCropCards(data) {
      const t = translations[currentLang];
      const container = document.getElementById('cropPricesContainer');
      container.innerHTML = data.map(crop => {
        const extraProfit = crop.fair - crop.middleman;
        const displayName = getCropName(crop);
        return `
        <div class="bg-[#fbf8f0] hover:bg-white border-2 border-stone-200 rounded-2xl p-3.5 flex items-center justify-between gap-3 transition shadow-xs hover:shadow-md">
          <div class="flex items-center gap-3">
            <img src="${crop.image}" alt="${displayName}" class="w-14 h-14 rounded-xl object-cover shadow-sm shrink-0 border-2 border-amber-500/30">
            <div>
              <div class="flex items-center gap-1.5">
                <h4 class="font-extrabold text-stone-900 text-xs sm:text-sm leading-snug">${displayName}</h4>
                <span class="text-[9px] font-bold px-1.5 py-0.2 rounded ${crop.trend.startsWith('+') ? 'bg-emerald-100 text-emerald-800' : 'bg-red-100 text-red-800'}">${crop.trend}</span>
              </div>
              <p class="text-[10px] font-bold text-stone-500 mt-0.5">${crop.category}</p>
              <div class="flex items-center gap-2 mt-1 text-xs">
                <span class="text-stone-400 line-through text-[11px]">₹${crop.middleman}</span>
                <span class="text-emerald-900 font-black">₹${crop.fair}<span class="text-[10px] text-stone-500 font-normal">/qtl</span></span>
              </div>
            </div>
          </div>
          <div class="text-right shrink-0">
            <span class="text-[10px] font-black text-emerald-800 bg-emerald-100 px-2 py-0.5 rounded-full block mb-1.5 border border-emerald-300">+₹${extraProfit} ${t.moreProfit}</span>
            <button onclick="quickSell('${crop.nameEn}', ${crop.fair})" class="text-[11px] font-black bg-amber-600 hover:bg-amber-700 active:scale-95 text-white px-3 py-1.5 rounded-xl shadow-xs transition">
              ${t.sellDirectBtn}
            </button>
          </div>
        </div>
      `}).join('');
    }

    function populateModalCropOptions() {
      const select = document.getElementById('modalCropName');
      select.innerHTML = allCrops.map(c => {
        const name = getCropName(c);
        return `<option value="${c.nameEn}">${name} (₹${c.fair}/qtl)</option>`;
      }).join('');
    }

    function onModalCropSelect(cropEnName) {
      const match = allCrops.find(c => c.nameEn === cropEnName);
      if (match) {
        document.getElementById('modalPrice').value = match.fair;
        if (!uploadedImageBase64) {
          document.getElementById('imagePreview').src = match.image;
          document.getElementById('imagePreviewContainer').classList.remove('hidden');
          document.getElementById('uploadPlaceholder').classList.add('hidden');
        }
      }
    }

    function filterCrops() {
      const query = document.getElementById('searchInput').value.toLowerCase();
      const filtered = allCrops.filter(c => {
        const matchesCategory = (activeCategory === 'All' || c.category === activeCategory);
        const matchesSearch = c.nameEn.toLowerCase().includes(query) || c.nameHi.includes(query) || c.category.toLowerCase().includes(query);
        return matchesCategory && matchesSearch;
      });
      renderCropCards(filtered);
    }

    function selectCategory(cat) {
      activeCategory = cat;
      renderCategoryTabs();
      filterCrops();
    }

    function scrollToSection(id) {
      const el = document.getElementById(id);
      if (el) el.scrollIntoView({ behavior: 'smooth' });
    }

    function previewCropImage(event) {
      const file = event.target.files[0];
      if (file) {
        const reader = new FileReader();
        reader.onload = function(e) {
          uploadedImageBase64 = e.target.result;
          document.getElementById('imagePreview').src = e.target.result;
          document.getElementById('imagePreviewContainer').classList.remove('hidden');
          document.getElementById('uploadPlaceholder').classList.add('hidden');
        };
        reader.readAsDataURL(file);
      }
    }

    function removeImage(event) {
      if (event) event.stopPropagation();
      uploadedImageBase64 = null;
      document.getElementById('cropImageInput').value = '';
      document.getElementById('imagePreview').src = '';
      document.getElementById('imagePreviewContainer').classList.add('hidden');
      document.getElementById('uploadPlaceholder').classList.remove('hidden');
    }

    // ================= AUTH LOGIC =================
    function switchAuthTab(mode) {
      authMode = mode;
      const t = translations[currentLang];
      const tabLogin = document.getElementById('tabLogin');
      const tabRegister = document.getElementById('tabRegister');
      const fieldFullName = document.getElementById('fieldFullName');
      const submitBtn = document.getElementById('authSubmitBtn');

      if (mode === 'login') {
        tabLogin.className = 'flex-1 py-2 rounded-xl bg-white shadow-sm text-emerald-900 font-bold transition';
        tabRegister.className = 'flex-1 py-2 rounded-xl text-stone-600 hover:text-stone-900 font-bold transition';
        fieldFullName.classList.add('hidden');
        document.getElementById('authName').required = false;
        submitBtn.innerText = t.loginBtn;
      } else {
        tabRegister.className = 'flex-1 py-2 rounded-xl bg-white shadow-sm text-emerald-900 font-bold transition';
        tabLogin.className = 'flex-1 py-2 rounded-xl text-stone-600 hover:text-stone-900 font-bold transition';
        fieldFullName.classList.remove('hidden');
        document.getElementById('authName').required = true;
        submitBtn.innerText = (currentLang === 'hi') ? "खाता बनाएं और प्रवेश करें" : "Create Account & Enter";
      }
    }

    function handleAuthSubmit(e) {
      e.preventDefault();
      const phone = document.getElementById('authPhone').value;
      const role = document.querySelector('input[name="authRole"]:checked').value;
      const name = authMode === 'register' ? document.getElementById('authName').value : (role === 'Farmer' ? 'किसान भाई' : 'खरीदार');
      loginUser({ name, phone, role });
    }

    function demoLogin() {
      loginUser({ name: (currentLang === 'hi') ? 'रामेश्वर पटेल (किसान)' : 'Ramesh Patel (Farmer)', phone: '919876543210', role: 'Farmer' });
    }

    function loginUser(userData) {
      currentUser = userData;
      localStorage.setItem('kisandirect_user', JSON.stringify(userData));
      document.getElementById('modalFarmer').value = userData.name;
      document.getElementById('modalWhatsApp').value = userData.phone.startsWith('91') ? userData.phone : '91' + userData.phone;
      document.getElementById('authScreen').classList.add('hidden');
      document.getElementById('mainApp').classList.remove('hidden');
    }

    function handleLogout() {
      localStorage.removeItem('kisandirect_user');
      currentUser = null;
      document.getElementById('authForm').reset();
      document.getElementById('mainApp').classList.add('hidden');
      document.getElementById('authScreen').classList.remove('hidden');
    }

    function checkExistingSession() {
      const saved = localStorage.getItem('kisandirect_user');
      if (saved) {
        try { loginUser(JSON.parse(saved)); } catch(e) { localStorage.removeItem('kisandirect_user'); }
      }
    }

    function openModal() {
      document.getElementById('harvestModal').classList.remove('hidden');
    }

    function closeModal() {
      document.getElementById('harvestModal').classList.add('hidden');
      removeImage();
    }

    function quickSell(cropEnName, fairPrice) {
      document.getElementById('modalCropName').value = cropEnName;
      document.getElementById('modalPrice').value = fairPrice;
      onModalCropSelect(cropEnName);
      openModal();
    }

    function handleFormSubmit(e) {
      e.preventDefault();
      const selectedCropEn = document.getElementById('modalCropName').value;
      const matchedCrop = allCrops.find(c => c.nameEn === selectedCropEn);
      const basePrice = parseInt(document.getElementById('modalPrice').value);

      const newBiddingLot = {
        id: Date.now(),
        cropEn: matchedCrop ? matchedCrop.nameEn : selectedCropEn,
        cropHi: matchedCrop ? matchedCrop.nameHi : selectedCropEn,
        farmer: document.getElementById('modalFarmer').value || (currentUser ? currentUser.name : "Farmer"),
        location: document.getElementById('modalLocation').value,
        qty: parseInt(document.getElementById('modalQuantity').value),
        basePrice: basePrice,
        currentBid: basePrice + 50,
        leadingBuyer: "Verified Buyer (Opening Bid)",
        leadingBuyerPhone: document.getElementById('modalWhatsApp').value,
        bidCount: 1,
        timeLeft: "04h 00m",
        image: uploadedImageBase64 || (matchedCrop ? matchedCrop.image : "https://images.unsplash.com/photo-1574323347407-f5e1ad6d020b?w=600&auto=format&fit=crop&q=80")
      };

      biddingListings.unshift(newBiddingLot);
      renderBiddingFloor();
      closeModal();
      document.getElementById('harvestForm').reset();
      removeImage();
      scrollToSection('sectionBidding');
    }

    // Startup Execution
    applyTranslations();
    checkExistingSession();
  </script>
</body>
</html>

