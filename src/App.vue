<script setup>
import { computed, ref } from 'vue'

const emit = defineEmits(['booking-submit'])

const menuOpen = ref(false)
const selectedService = ref('')
const selectedDoctor = ref('')
const selectedDate = ref('')
const selectedTime = ref('')
const fullName = ref('')
const phone = ref('')
const submitted = ref(false)
const submitting = ref(false)

const services = [
  {
    icon: '✧',
    title: 'طراحی لبخند',
    desc: 'طراحی لبخندی طبیعی و هماهنگ با چهره شما',
    image: '/image/Digital Smile Design Consultation.png',
    tag: 'زیبایی',
  },
  {
    icon: '◈',
    title: 'ایمپلنت دندان',
    desc: 'بازگرداندن زیبایی و عملکرد طبیعی دندان‌ها',
    image: '/image/Dental Implant Model in Modern Clinic.png',
    tag: 'درمانی',
  },
  {
    icon: '✦',
    title: 'لمینت و کامپوزیت',
    desc: 'اصلاح فرم و رنگ دندان با طرح اختصاصی',
    image: '/image/Dental Veneer Shade Matching Consultation.png',
    tag: 'زیبایی',
  },
  {
    icon: '◇',
    title: 'ارتودنسی',
    desc: 'ردیف‌کردن دندان‌ها با روش‌های تخصصی',
    image: '/image/Smiling Patient During Braces Adjustment.png',
    tag: 'درمانی',
  },
  {
    icon: '✧',
    title: 'جرم‌گیری و ترمیم',
    desc: 'مراقبت حرفه‌ای برای حفظ سلامت دهان',
    image: '/image/Professional Dental Cleaning in a Modern Clinic.png',
    tag: 'مراقبتی',
  },
  {
    icon: '◈',
    title: 'درمان ریشه',
    desc: 'درمان دقیق و اصولی برای حفظ دندان طبیعی',
    image: '/image/Bright Dental Cleaning Examination.png',
    tag: 'درمانی',
  },
]

const doctors = [
  {
    name: 'دکتر آرمان رضایی',
    specialty: 'متخصص ایمپلنت و جراحی',
    image: '/image/Confident Dentist in Modern Clinic.png',
    initials: 'آر',
  },
  {
    name: 'دکتر سارا محمدی',
    specialty: 'متخصص زیبایی و ترمیمی',
    image: '/image/Confident Dentist in a Modern Clinic.png',
    initials: 'سم',
  },
  {
    name: 'دکتر علی کریمی',
    specialty: 'متخصص ارتودنسی',
    image: '/image/Confident Dentist in a Modern Clinic (1).png',
    initials: 'عک',
  },
]

const times = ['09:00', '10:00', '11:00', '12:00', '14:00', '15:00', '16:00', '17:00']
const today = computed(() => {
  const now = new Date()
  const local = new Date(now.getTime() - now.getTimezoneOffset() * 60000)
  return local.toISOString().slice(0, 10)
})

const canSubmit = computed(
  () =>
    fullName.value.trim().length >= 3 &&
    /^09\d{9}$/.test(phone.value.trim()) &&
    selectedService.value &&
    selectedDoctor.value &&
    selectedDate.value &&
    selectedTime.value,
)

function chooseService(service) {
  selectedService.value = service
  document.getElementById('booking')?.scrollIntoView({ behavior: 'smooth' })
}

function chooseDoctor(doctor) {
  selectedDoctor.value = doctor
  document.getElementById('booking')?.scrollIntoView({ behavior: 'smooth' })
}

function submitBooking() {
  if (!canSubmit.value || submitting.value) return
  submitting.value = true

  const booking = {
    name: fullName.value.trim(),
    phone: phone.value.trim(),
    service: selectedService.value,
    doctor: selectedDoctor.value,
    date: selectedDate.value,
    time: selectedTime.value,
  }

  // The parent application can send this payload to its API.
  emit('booking-submit', booking)
  submitted.value = true
  submitting.value = false
}

function resetBooking() {
  submitted.value = false
  fullName.value = ''
  phone.value = ''
  selectedService.value = ''
  selectedDoctor.value = ''
  selectedDate.value = ''
  selectedTime.value = ''
}

function closeMenu() {
  menuOpen.value = false
}
</script>

<template>
  <main dir="rtl" class="site-shell">
    <header class="site-header">
      <nav class="nav-wrap" aria-label="ناوبری اصلی">
        <a class="brand" href="#home" @click="closeMenu" aria-label="دنتاکر، صفحه اصلی">
          <span class="brand-mark">D</span>
          <span class="brand-copy">
            <strong>DENTA<span>CARE</span></strong>
            <small>PREMIUM DENTAL STUDIO</small>
          </span>
        </a>

        <div class="desktop-nav">
          <a href="#home">خانه</a>
          <a href="#about">درباره ما</a>
          <a href="#services">خدمات</a>
          <a href="#doctors">پزشکان</a>
          <a href="#contact">تماس با ما</a>
        </div>

        <a class="button button-primary nav-cta" href="#booking">
          رزرو نوبت <span aria-hidden="true">↗</span>
        </a>

        <button
          class="menu-toggle"
          type="button"
          :aria-expanded="menuOpen"
          aria-label="باز و بسته کردن منو"
          @click="menuOpen = !menuOpen"
        >
          <span></span><span></span><span></span>
        </button>
      </nav>

      <Transition name="drop">
        <div v-if="menuOpen" class="mobile-nav">
          <a href="#home" @click="closeMenu">خانه</a>
          <a href="#about" @click="closeMenu">درباره ما</a>
          <a href="#services" @click="closeMenu">خدمات</a>
          <a href="#doctors" @click="closeMenu">پزشکان</a>
          <a href="#contact" @click="closeMenu">تماس با ما</a>
          <a class="mobile-booking" href="#booking" @click="closeMenu">رزرو نوبت آنلاین</a>
        </div>
      </Transition>
    </header>

    <section id="home" class="hero">
      <div class="hero-image" aria-hidden="true"></div>
      <div class="hero-shade" aria-hidden="true"></div>
      <div class="hero-orb hero-orb-one" aria-hidden="true"></div>
      <div class="hero-orb hero-orb-two" aria-hidden="true"></div>

      <div class="container hero-grid">
        <div class="hero-copy">
          <div class="eyebrow eyebrow-dark">
            <span class="live-dot"></span>
            YOUR SMILE, OUR PRIORITY
          </div>
          <h1>مراقبت<br /><em>فراتر از انتظار</em></h1>
          <p>
            در دنتاکر، دانش پزشکی با نگاه زیبایی‌شناسانه همراه می‌شود تا تجربه‌ای مطمئن، آرام و
            حرفه‌ای از مراقبت دندان داشته باشید.
          </p>
          <div class="hero-actions">
            <a class="button button-primary button-large" href="#booking">
              همین حالا نوبت بگیر <span aria-hidden="true">↗</span>
            </a>
            <a class="button button-glass button-large" href="#services"> مشاهده خدمات </a>
          </div>
          <div class="hero-proof">
            <div class="proof-avatars" aria-hidden="true">
              <img
                src="https://images.unsplash.com/photo-1559839734-2b71ea197ec2?w=100&h=100&fit=crop&crop=faces"
                alt=""
              />
              <img
                src="https://images.unsplash.com/photo-1622253692010-333f2da6031d?w=100&h=100&fit=crop&crop=faces"
                alt=""
              />
              <img
                src="https://images.unsplash.com/photo-1612349317150-e413f6a5b16d?w=100&h=100&fit=crop&crop=faces"
                alt=""
              />
            </div>
            <div>
              <div class="stars" aria-label="پنج ستاره">★★★★★</div>
              <span>همراهی تیمی از متخصصان دندان‌پزشکی</span>
            </div>
          </div>
        </div>

        <div class="hero-feature">
          <div class="hero-feature-image">
            <img
              src="/image/Modern Dental Care Examination (1).png"
              alt="معاینه تخصصی در کلینیک دنتاکر"
            />
            <span class="image-chip"><i></i> مراقبت با استاندارد حرفه‌ای</span>
          </div>
          <div class="hero-feature-bottom">
            <div>
              <small>YOUR SMILE, OUR PRIORITY</small>
              <strong>مراقبت فراتر از انتظار</strong>
            </div>
            <span class="feature-spark">✦</span>
          </div>
        </div>
      </div>

      <div class="hero-bottom container">
        <span>SCROLL TO DISCOVER</span>
        <span class="scroll-line"></span>
        <span>01 / 06</span>
      </div>
    </section>

    <section class="trust-strip" aria-label="ویژگی‌های کلینیک">
      <div class="container trust-grid">
        <article class="trust-item">
          <span class="trust-number">01</span>
          <div><strong>تجهیزات مدرن</strong><small>فناوری روز دندان‌پزشکی</small></div>
        </article>
        <article class="trust-item">
          <span class="trust-number">02</span>
          <div><strong>تیم متخصص</strong><small>پزشکان حرفه‌ای و باتجربه</small></div>
        </article>
        <article class="trust-item">
          <span class="trust-number">03</span>
          <div><strong>محیط آرام</strong><small>تجربه درمان با آرامش بیشتر</small></div>
        </article>
        <article class="trust-item">
          <span class="trust-number">04</span>
          <div><strong>مراقبت اختصاصی</strong><small>توجه به نیاز هر مراجعه‌کننده</small></div>
        </article>
      </div>
    </section>

    <section id="about" class="section about-section">
      <div class="container about-grid">
        <div class="about-visual">
          <img
            class="about-main-image object-cover"
            src="/image/Modern Teal Dental Clinic Interior.png"
            alt="فضای داخلی مدرن کلینیک دنتاکر"
          />
        </div>

        <div class="about-copy">
          <span class="section-kicker">درباره کلینیک ما</span>
          <h2 class="section-title">جایی برای خلق<br /><span>لبخندهای ماندگار</span></h2>
          <p class="section-description">
            ما باور داریم دندان‌پزشکی فقط یک درمان نیست؛ تجربه‌ای از اعتماد، آرامش و توجه به جزئیات
            است. در دنتاکر تلاش می‌کنیم هر مراجعه‌کننده مسیر درمانی روشن و متناسب با نیاز خود را
            تجربه کند.
          </p>
          <div class="about-points">
            <div class="about-point">
              <span>✓</span>
              <div><strong>مشاوره شفاف</strong><small>توضیح مراحل درمان پیش از شروع</small></div>
            </div>
            <div class="about-point">
              <span>✓</span>
              <div>
                <strong>برنامه درمان اختصاصی</strong><small>انتخاب روش متناسب با شرایط شما</small>
              </div>
            </div>
            <div class="about-point">
              <span>✓</span>
              <div>
                <strong>توجه به تجربه مراجعه‌کننده</strong
                ><small>محیطی آرام و ارتباطی محترمانه</small>
              </div>
            </div>
          </div>
          <a class="text-link" href="#booking">برای مشاوره اقدام کنید <span>←</span></a>
        </div>
      </div>
    </section>

    <section id="services" class="section services-section">
      <div class="container">
        <div class="section-heading">
          <div>
            <span class="section-kicker">خدمات تخصصی دنتاکر</span>
            <h2 class="section-title">مراقبت از لبخند شما،<br /><span>با نگاهی دقیق‌تر</span></h2>
          </div>
          <p class="section-description">
            از مراقبت‌های پیشگیرانه تا درمان‌های زیبایی و تخصصی؛ خدمات مورد نیازتان را با راهنمایی
            تیم درمان انتخاب کنید.
          </p>
        </div>

        <div class="services-grid">
          <article v-for="(service, index) in services" :key="service.title" class="service-card">
            <div class="service-image">
              <img :src="service.image" :alt="service.title" loading="lazy" />
              <span class="service-index">0{{ index + 1 }}</span>
              <span class="service-tag">{{ service.tag }}</span>
            </div>
            <div class="service-content">
              <div class="service-title-row">
                <span class="service-icon">{{ service.icon }}</span>
                <h3>{{ service.title }}</h3>
              </div>
              <p>{{ service.desc }}</p>
              <button class="service-link" type="button" @click="chooseService(service.title)">
                درخواست این خدمت <span>←</span>
              </button>
            </div>
          </article>
        </div>
      </div>
    </section>

    <section class="statement-section">
      <div class="statement-bg" aria-hidden="true"></div>
      <div class="container statement-content">
        <span class="section-kicker section-kicker-light">YOUR SMILE MATTERS</span>
        <h2>لبخندت را با خیال آسوده<br /><span>به ما بسپار.</span></h2>
        <p>برای شروع مسیر مراقبت از لبخندتان، یک قدم کافی‌ست.</p>
        <a class="button button-primary button-large" href="#booking"
          >رزرو نوبت آنلاین <span>↗</span></a
        >
      </div>
      <span class="statement-watermark" aria-hidden="true">D</span>
    </section>

    <section id="doctors" class="section doctors-section">
      <div class="container">
        <div class="center-heading">
          <span class="section-kicker">تیم پزشکی</span>
          <h2 class="section-title">متخصصان همراه <span>شما</span></h2>
          <p class="section-description">دانش، تجربه و تعهد در کنار توجه به نیازهای شما</p>
        </div>

        <div class="doctors-grid">
          <article v-for="doctor in doctors" :key="doctor.name" class="doctor-card">
            <div class="doctor-photo">
              <img :src="doctor.image" :alt="doctor.name" loading="lazy" />
              <span class="doctor-monogram">{{ doctor.initials }}</span>
            </div>
            <div class="doctor-info">
              <span class="doctor-label">DENTAL SPECIALIST</span>
              <h3>{{ doctor.name }}</h3>
              <p>{{ doctor.specialty }}</p>
              <button type="button" class="doctor-book" @click="chooseDoctor(doctor.name)">
                رزرو نوبت با پزشک <span>←</span>
              </button>
            </div>
          </article>
        </div>
      </div>
    </section>

    <section id="booking" class="booking-section">
      <div class="booking-decoration booking-decoration-one" aria-hidden="true"></div>
      <div class="booking-decoration booking-decoration-two" aria-hidden="true"></div>
      <div class="container booking-container">
        <div class="booking-heading">
          <span class="section-kicker section-kicker-light">ONLINE APPOINTMENT</span>
          <h2>شروع یک لبخند تازه</h2>
          <p>اطلاعات خود را وارد کنید تا برای هماهنگی نوبت با شما تماس بگیریم.</p>
        </div>

        <div class="booking-panel">
          <div v-if="submitted" class="success-state" role="status" aria-live="polite">
            <div class="success-icon">✓</div>
            <span class="section-kicker">درخواست شما دریافت شد</span>
            <h3>ممنون از اعتمادتان، {{ fullName }} عزیز.</h3>
            <p>
              درخواست شما برای <strong>{{ selectedService }}</strong> با
              <strong>{{ selectedDoctor }}</strong> در تاریخ
              <strong>{{ selectedDate }}</strong> ساعت <strong>{{ selectedTime }}</strong> ثبت شد.
              برای تأیید نهایی، کلینیک با شما تماس خواهد گرفت.
            </p>
            <button class="button button-primary button-large" type="button" @click="resetBooking">
              ثبت درخواست جدید
            </button>
          </div>

          <form v-else class="booking-form" @submit.prevent="submitBooking">
            <div class="form-intro">
              <span class="form-step">01 <i></i> 02</span>
              <div>
                <h3>فرم درخواست نوبت</h3>
                <p>فیلدهای ستاره‌دار الزامی هستند.</p>
              </div>
            </div>

            <div class="form-section-title"><span>۱</span> اطلاعات شما</div>
            <div class="form-grid">
              <label class="field">
                <span>نام و نام خانوادگی <b>*</b></span>
                <input
                  v-model="fullName"
                  type="text"
                  autocomplete="name"
                  minlength="3"
                  required
                  placeholder="مثلاً سارا احمدی"
                />
              </label>
              <label class="field">
                <span>شماره موبایل <b>*</b></span>
                <input
                  v-model="phone"
                  type="tel"
                  inputmode="numeric"
                  autocomplete="tel"
                  maxlength="11"
                  pattern="09[0-9]{9}"
                  required
                  placeholder="09123456789"
                  dir="ltr"
                />
                <small>شماره موبایل را با ۰۹ وارد کنید.</small>
              </label>
            </div>

            <div class="form-section-title"><span>۲</span> جزئیات نوبت</div>
            <div class="form-grid">
              <label class="field">
                <span>خدمت مورد نظر <b>*</b></span>
                <select v-model="selectedService" required>
                  <option disabled value="">یک خدمت انتخاب کنید</option>
                  <option v-for="service in services" :key="service.title" :value="service.title">
                    {{ service.title }}
                  </option>
                </select>
              </label>
              <label class="field">
                <span>پزشک مورد نظر <b>*</b></span>
                <select v-model="selectedDoctor" required>
                  <option disabled value="">یک پزشک انتخاب کنید</option>
                  <option v-for="doctor in doctors" :key="doctor.name" :value="doctor.name">
                    {{ doctor.name }}
                  </option>
                </select>
              </label>
              <label class="field">
                <span>تاریخ مراجعه <b>*</b></span>
                <input v-model="selectedDate" type="date" :min="today" required />
              </label>
              <label class="field">
                <span>ساعت پیشنهادی <b>*</b></span>
                <select v-model="selectedTime" required>
                  <option disabled value="">ساعت را انتخاب کنید</option>
                  <option v-for="time in times" :key="time" :value="time">{{ time }}</option>
                </select>
              </label>
            </div>

            <div class="form-footer">
              <p>
                <span>ⓘ</span> ثبت این فرم به معنی درخواست نوبت است؛ زمان مراجعه پس از تماس کلینیک
                قطعی می‌شود.
              </p>
              <button
                class="button button-primary button-large submit-button"
                type="submit"
                :disabled="!canSubmit || submitting"
              >
                {{ submitting ? 'در حال ثبت...' : 'ثبت درخواست نوبت' }} <span>↗</span>
              </button>
            </div>
          </form>
        </div>
      </div>
    </section>

    <section id="contact" class="section contact-section">
      <div class="container contact-grid">
        <div class="contact-copy">
          <span class="section-kicker">ارتباط با ما</span>
          <h2 class="section-title">برای پاسخ به پرسش‌هایتان<br /><span>کنار شما هستیم.</span></h2>
          <p class="section-description">
            برای دریافت اطلاعات بیشتر یا هماهنگی مراجعه، از راه‌های ارتباطی زیر با ما در تماس باشید.
          </p>

          <div class="contact-list">
            <div class="contact-row">
              <span class="contact-icon">⌖</span>
              <div><small>آدرس کلینیک</small><strong>تهران، آدرس کلینیک شما</strong></div>
            </div>
            <div class="contact-row">
              <span class="contact-icon">☎</span>
              <div><small>شماره تماس</small><strong dir="ltr">021-12345678</strong></div>
            </div>
            <div class="contact-row">
              <span class="contact-icon">◷</span>
              <div><small>ساعات کاری</small><strong>شنبه تا پنجشنبه، ۹ تا ۲۰</strong></div>
            </div>
          </div>
        </div>
        <div class="contact-visual">
          <img
            src="/image/Modern DentaCare Clinic Branding.png"
            alt="فضای کلینیک دنتاکر"
            loading="lazy"
          />
          <div class="contact-image-caption">
            <span>✦</span>
            <div><strong>DENTACARE</strong><small>Care that makes you smile</small></div>
          </div>
        </div>
      </div>
    </section>

    <footer class="site-footer">
      <div class="container">
        <div class="footer-main">
          <div class="footer-brand">
            <a class="brand brand-footer" href="#home">
              <span class="brand-mark">D</span>
              <span class="brand-copy"
                ><strong>DENTA<span>CARE</span></strong
                ><small>PREMIUM DENTAL STUDIO</small></span
              >
            </a>
            <p>زیبایی لبخند شما، حاصل تخصص، دقت و مراقبت ماست.</p>
          </div>
          <div class="footer-links">
            <h3>دسترسی سریع</h3>
            <a href="#about">درباره ما</a>
            <a href="#services">خدمات تخصصی</a>
            <a href="#doctors">تیم پزشکی</a>
            <a href="#booking">رزرو نوبت</a>
          </div>
          <div class="footer-contact">
            <h3>با ما در ارتباط باشید</h3>
            <p>تهران، آدرس کلینیک شما</p>
            <p dir="ltr">021-12345678</p>
            <a href="#contact">اطلاعات تماس <span>←</span></a>
          </div>
        </div>
        <div class="footer-bottom">
          <span>© 2026 DentaCare. تمامی حقوق محفوظ است.</span>
          <a href="#home">بازگشت به بالا ↑</a>
        </div>
      </div>
    </footer>
  </main>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Vazirmatn:wght@300;400;500;600;700;800;900&display=swap');

:root {
  font-family: 'Vazirmatn', sans-serif;
  color: #315d7d;
  background: #f8fbfa;
  font-synthesis: none;
  text-rendering: optimizeLegibility;
  scroll-behavior: smooth;
  scroll-padding-top: 100px;
  --navy: #397fb7;
  --navy-deep: #eaf5ff;
  --teal: #68afe8;
  --teal-dark: #397fb7;
  --mint: #b8ddfa;
  --ink: #315d7d;
  --muted: #7896ad;
  --paper: #f8fcff;
  --line: #e3f0fa;
  --shadow: 0 22px 65px rgba(16, 44, 64, 0.09);
}
* {
  box-sizing: border-box;
}
html {
  scroll-behavior: smooth;
}
body {
  margin: 0;
  min-width: 320px;
  font-family: 'Vazirmatn', sans-serif;
}
button,
input,
select {
  font: inherit;
}
button,
a {
  -webkit-tap-highlight-color: transparent;
}
a {
  color: inherit;
  text-decoration: none;
}
button {
  cursor: pointer;
}
img {
  display: block;
  max-width: 100%;
}
::selection {
  color: white;
  background: var(--teal);
}
:focus-visible {
  outline: 3px solid var(--mint);
  outline-offset: 4px;
}
.container {
  width: min(1160px, calc(100% - 48px));
  margin-inline: auto;
}
.site-shell {
  overflow: clip;
  background: var(--paper);
}
.site-header {
  position: absolute;
  z-index: 20;
  inset: 0 0 auto;
  padding: 18px 24px 0;
}
.nav-wrap {
  width: min(1260px, 100%);
  height: 78px;
  margin: auto;
  padding: 0 24px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 28px;
  border: 1px solid rgba(255, 255, 255, 0.76);
  border-radius: 20px;
  background: rgba(255, 255, 255, 0.91);
  box-shadow: 0 12px 40px rgba(4, 24, 38, 0.12);
  backdrop-filter: blur(22px);
}
.brand {
  display: inline-flex;
  align-items: center;
  gap: 11px;
  flex: 0 0 auto;
}
.brand-mark {
  width: 44px;
  height: 44px;
  display: grid;
  place-items: center;
  border-radius: 14px;
  color: white;
  font-size: 25px;
  font-weight: 900;
  background: linear-gradient(145deg, #1bb9a9, #087b82);
  box-shadow: 0 8px 22px #0c9b8e40;
}
.brand-copy {
  display: grid;
  gap: 1px;
}
.brand-copy strong {
  font-size: 20px;
  letter-spacing: -0.7px;
  line-height: 1.2;
  color: var(--navy);
}
.brand-copy strong span {
  color: var(--teal);
}
.brand-copy small {
  color: #8a9aa3;
  font:
    700 8px/1.5 Arial,
    sans-serif;
  letter-spacing: 2px;
}
.desktop-nav {
  display: flex;
  align-items: center;
  gap: clamp(18px, 3vw, 38px);
  color: #536875;
  font-size: 13px;
  font-weight: 700;
}
.desktop-nav a {
  position: relative;
  padding-block: 8px;
  transition: color 0.2s;
}
.desktop-nav a::after {
  content: '';
  position: absolute;
  right: 0;
  bottom: 1px;
  width: 0;
  height: 2px;
  border-radius: 5px;
  background: var(--teal);
  transition: width 0.25s;
}
.desktop-nav a:hover {
  color: var(--teal-dark);
}
.desktop-nav a:hover::after {
  width: 100%;
}
.button {
  min-height: 46px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
  gap: 12px;
  padding: 0 22px;
  border: 0;
  border-radius: 13px;
  font-size: 13px;
  font-weight: 800;
  transition:
    transform 0.25s,
    box-shadow 0.25s,
    background 0.25s;
}
.button-primary {
  color: white;
  background: linear-gradient(135deg, #12ad9e, #087d73);
  box-shadow: 0 10px 25px #0c9b8e35;
}
.button-primary:hover:not(:disabled) {
  transform: translateY(-3px);
  box-shadow: 0 16px 32px #0c9b8e4a;
}
.button-primary:disabled {
  cursor: not-allowed;
  opacity: 0.45;
  box-shadow: none;
}
.button-large {
  min-height: 56px;
  padding-inline: 27px;
  border-radius: 15px;
}
.button-glass {
  color: white;
  border: 1px solid #ffffff55;
  background: #ffffff12;
  backdrop-filter: blur(10px);
}
.button-glass:hover {
  color: var(--navy);
  background: white;
}
.menu-toggle {
  display: none;
  width: 44px;
  height: 44px;
  padding: 11px;
  border: 1px solid var(--line);
  border-radius: 12px;
  background: white;
}
.menu-toggle span {
  display: block;
  height: 2px;
  margin: 4px 0;
  border-radius: 2px;
  background: var(--navy);
}
.mobile-nav {
  display: none;
}
.hero {
  position: relative;
  isolation: isolate;
  min-height: 790px;
  display: flex;
  align-items: center;
  padding: 140px 0 105px;
  color: white;
  background: var(--navy);
}
.hero-image,
.hero-shade {
  position: absolute;
  z-index: -2;
  inset: 0;
}
.hero-image {
  background: url('https://images.unsplash.com/photo-1629909613654-28e377c37b09?w=2000&q=90')
    center/cover;
  transform: scale(1.025);
  animation: hero-drift 22s ease-in-out infinite alternate;
}
.hero-shade {
  z-index: -1;
  background: linear-gradient(
    270deg,
    rgba(8, 31, 48, 0.97) 0%,
    rgba(13, 43, 61, 0.88) 43%,
    rgba(13, 43, 61, 0.38) 100%
  );
}
.hero-orb {
  position: absolute;
  z-index: -1;
  width: 330px;
  aspect-ratio: 1;
  border-radius: 50%;
  background: #17b5a433;
  filter: blur(85px);
  pointer-events: none;
}
.hero-orb-one {
  left: 7%;
  top: 17%;
}
.hero-orb-two {
  right: 42%;
  bottom: 4%;
  width: 220px;
  background: #8be6d31c;
}
.hero-grid {
  display: grid;
  grid-template-columns: 1.1fr 0.9fr;
  align-items: center;
  gap: 75px;
}
.hero-copy {
  max-width: 610px;
  animation: fade-up 0.8s both;
}
.eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 11px;
  padding: 10px 16px;
  border: 1px solid #ffffff32;
  border-radius: 99px;
  font-size: 11px;
  font-weight: 600;
}
.eyebrow-dark {
  background: #ffffff12;
  backdrop-filter: blur(12px);
}
.live-dot {
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: var(--mint);
  box-shadow: 0 0 0 5px #80e1d31c;
}
.hero h1 {
  margin: 25px 0 18px;
  font-size: clamp(48px, 6.1vw, 76px);
  font-weight: 900;
  line-height: 1.38;
  letter-spacing: -2px;
}
.hero h1 em {
  color: var(--mint);
  font-style: normal;
}
.hero-copy > p {
  max-width: 510px;
  margin: 0;
  color: #c2d0d7;
  font-size: 14px;
  line-height: 2.35;
}
.hero-actions {
  display: flex;
  flex-wrap: wrap;
  gap: 13px;
  margin-top: 32px;
}
.hero-proof {
  display: flex;
  align-items: center;
  gap: 15px;
  margin-top: 42px;
}
.proof-avatars {
  display: flex;
  padding-right: 5px;
}
.proof-avatars img {
  width: 38px;
  height: 38px;
  margin-right: -7px;
  border: 2px solid var(--navy);
  border-radius: 50%;
  object-fit: cover;
}
.stars {
  color: #f7c86a;
  font-size: 13px;
  letter-spacing: 2px;
}
.hero-proof span {
  display: block;
  margin-top: 3px;
  color: #b9c8d0;
  font-size: 10px;
}
.hero-feature {
  width: min(340px, 100%);
  justify-self: end;
  padding: 13px;
  border: 1px solid #ffffff45;
  border-radius: 25px;
  background: linear-gradient(145deg, #ffffff2a, #ffffff0b);
  box-shadow: 0 28px 80px #0004;
  backdrop-filter: blur(18px);
  transform: rotate(-2deg);
  animation: float 6s ease-in-out infinite;
}
.hero-feature-image {
  position: relative;
  height: 245px;
  overflow: hidden;
  border-radius: 17px;
}
.hero-feature-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.7s;
}
.hero-feature:hover .hero-feature-image img {
  transform: scale(1.06);
}
.image-chip {
  position: absolute;
  right: 12px;
  bottom: 12px;
  display: flex;
  align-items: center;
  gap: 7px;
  padding: 8px 11px;
  border: 1px solid #ffffff70;
  border-radius: 30px;
  color: white;
  background: #102c40a8;
  font-size: 9px;
  backdrop-filter: blur(10px);
}
.image-chip i {
  width: 6px;
  height: 6px;
  border-radius: 50%;
  background: var(--mint);
}
.hero-feature-bottom {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 17px 7px 6px;
}
.hero-feature-bottom div {
  display: grid;
  gap: 7px;
}
.hero-feature-bottom small {
  color: var(--mint);
  font:
    700 8px Arial,
    sans-serif;
  letter-spacing: 1.2px;
}
.hero-feature-bottom strong {
  font-size: 13px;
}
.feature-spark {
  width: 40px;
  height: 40px;
  display: grid;
  place-items: center;
  border-radius: 13px;
  color: var(--navy);
  background: var(--mint);
  font-size: 20px;
}
.hero-bottom {
  position: absolute;
  right: 50%;
  bottom: 27px;
  display: flex;
  align-items: center;
  gap: 13px;
  transform: translateX(50%);
  color: #ffffff8a;
  font:
    700 8px Arial,
    sans-serif;
  letter-spacing: 1.6px;
}
.scroll-line {
  width: 70px;
  height: 1px;
  background: #ffffff55;
}
.trust-strip {
  position: relative;
  z-index: 2;
  margin-top: -1px;
  padding: 34px 0;
  border-bottom: 1px solid var(--line);
  background: #fff;
}
.trust-grid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}
.trust-item {
  display: flex;
  align-items: center;
  gap: 15px;
  padding: 9px 15px;
  border-left: 1px solid var(--line);
}
.trust-item:last-child {
  border: 0;
}
.trust-number {
  color: var(--teal);
  font:
    800 12px Arial,
    sans-serif;
}
.trust-item div {
  display: grid;
  gap: 5px;
}
.trust-item strong {
  font-size: 13px;
}
.trust-item small {
  color: #91a0a8;
  font-size: 10px;
}
.section {
  padding: 112px 0;
}
.about-grid {
  display: grid;
  grid-template-columns: 0.95fr 1.05fr;
  align-items: center;
  gap: 95px;
}
.about-visual {
  position: relative;
  padding: 0 18px 22px 0;
}
.about-outline {
  position: absolute;
  top: -18px;
  right: -3px;
  width: 42%;
  height: 42%;
  border: 1px solid #9ddbd3;
  border-radius: 25px;
}
.about-main-image {
  position: relative;
  width: 100%;
  height: 480px;
  border-radius: 24px;
  object-fit: cover;
  box-shadow: var(--shadow);
}
.about-note {
  position: absolute;
  left: -22px;
  bottom: 0;
  display: flex;
  align-items: center;
  gap: 12px;
  padding: 17px 20px;
  border: 1px solid #edf3f2;
  border-radius: 16px;
  background: white;
  box-shadow: 0 15px 40px #102c4018;
}
.note-icon {
  width: 38px;
  height: 38px;
  display: grid;
  place-items: center;
  border-radius: 12px;
  color: white;
  background: var(--teal);
}
.about-note div {
  display: grid;
  gap: 5px;
}
.about-note strong {
  font-size: 11px;
}
.about-note small {
  color: var(--muted);
  font-size: 9px;
}
.about-side-label {
  position: absolute;
  right: -23px;
  bottom: 85px;
  color: #9aadb4;
  font:
    700 8px Arial,
    sans-serif;
  letter-spacing: 2px;
  writing-mode: vertical-rl;
  transform: rotate(180deg);
}
.section-kicker {
  display: inline-flex;
  align-items: center;
  gap: 9px;
  color: var(--teal-dark);
  font-size: 12px;
  font-weight: 900;
}
.section-kicker::before {
  content: '';
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: var(--teal);
  box-shadow: 0 0 0 5px #0c9b8e15;
}
.section-title {
  margin: 17px 0 0;
  color: var(--navy);
  font-size: clamp(30px, 4vw, 47px);
  font-weight: 900;
  line-height: 1.65;
  letter-spacing: -1px;
}
.section-title span {
  color: var(--teal);
}
.section-description {
  color: var(--muted);
  font-size: 13px;
  line-height: 2.4;
}
.about-copy .section-description {
  max-width: 540px;
  margin-top: 18px;
}
.about-points {
  display: grid;
  gap: 17px;
  margin-top: 27px;
}
.about-point {
  display: flex;
  align-items: center;
  gap: 12px;
}
.about-point > span {
  width: 25px;
  height: 25px;
  display: grid;
  flex: 0 0 25px;
  place-items: center;
  border-radius: 50%;
  color: var(--teal-dark);
  background: #e4f6f2;
  font-size: 12px;
  font-weight: 900;
}
.about-point div {
  display: grid;
  gap: 4px;
}
.about-point strong {
  font-size: 12px;
}
.about-point small {
  color: #91a0a8;
  font-size: 10px;
}
.text-link {
  display: inline-flex;
  align-items: center;
  gap: 12px;
  margin-top: 31px;
  padding-bottom: 7px;
  border-bottom: 1px solid #9ed9d1;
  color: var(--teal-dark);
  font-size: 12px;
  font-weight: 900;
}
.text-link span {
  transition: transform 0.2s;
}
.text-link:hover span {
  transform: translateX(-5px);
}
.services-section {
  position: relative;
  isolation: isolate;
  background: #f0f7f6;
}
.services-section::before {
  content: '';
  position: absolute;
  z-index: -1;
  inset: 0;
  opacity: 0.55;
  background-image: radial-gradient(#a8d8d1 1px, transparent 1px);
  background-size: 24px 24px;
  mask-image: linear-gradient(90deg, black, transparent 60%);
}
.section-heading {
  display: flex;
  align-items: end;
  justify-content: space-between;
  gap: 35px;
  margin-bottom: 44px;
}
.section-heading .section-description {
  max-width: 350px;
  margin-bottom: 8px;
}
.services-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 22px;
}
.service-card {
  overflow: hidden;
  border: 1px solid #e3edeb;
  border-radius: 21px;
  background: white;
  box-shadow: 0 9px 28px #102c4008;
  transition:
    transform 0.35s,
    box-shadow 0.35s,
    border-color 0.35s;
}
.service-card:hover {
  transform: translateY(-8px);
  border-color: #b9e2dc;
  box-shadow: 0 24px 50px #102c4018;
}
.service-image {
  position: relative;
  height: 205px;
  overflow: hidden;
  background: #e3efed;
}
.service-image::after {
  content: '';
  position: absolute;
  inset: 35% 0 0;
  background: linear-gradient(transparent, #102c4040);
}
.service-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.7s;
}
.service-card:hover .service-image img {
  transform: scale(1.07);
}
.service-index {
  position: absolute;
  z-index: 1;
  top: 15px;
  left: 15px;
  color: white;
  font:
    800 11px Arial,
    sans-serif;
  text-shadow: 0 2px 8px #0007;
}
.service-tag {
  position: absolute;
  z-index: 1;
  right: 15px;
  bottom: 14px;
  padding: 6px 11px;
  border: 1px solid #ffffff80;
  border-radius: 20px;
  color: white;
  background: #102c4070;
  font-size: 9px;
  backdrop-filter: blur(8px);
}
.service-content {
  padding: 20px 21px 21px;
}
.service-title-row {
  display: flex;
  align-items: center;
  gap: 10px;
}
.service-icon {
  width: 35px;
  height: 35px;
  display: grid;
  place-items: center;
  border-radius: 11px;
  color: var(--teal-dark);
  background: #eaf7f4;
  font-size: 18px;
}
.service-title-row h3 {
  margin: 0;
  color: var(--navy);
  font-size: 15px;
  font-weight: 900;
}
.service-content p {
  min-height: 43px;
  margin: 12px 0 16px;
  color: #8999a1;
  font-size: 11px;
  line-height: 2;
}
.service-link {
  display: inline-flex;
  align-items: center;
  gap: 10px;
  padding: 0;
  border: 0;
  color: var(--teal-dark);
  background: transparent;
  font-size: 11px;
  font-weight: 900;
}
.service-link span {
  transition: transform 0.2s;
}
.service-link:hover span {
  transform: translateX(-5px);
}
.statement-section {
  position: relative;
  isolation: isolate;
  overflow: hidden;
  padding: 100px 0;
  color: white;
  background: var(--navy);
  text-align: center;
}
.statement-bg {
  position: absolute;
  z-index: -2;
  inset: 0;
  background:
    linear-gradient(90deg, #0b2538e8, #102c40d9),
    url('https://images.unsplash.com/photo-1629909613654-28e377c37b09?w=1800&q=85') center/cover;
}
.statement-section::after {
  content: '';
  position: absolute;
  z-index: -1;
  top: -220px;
  left: 12%;
  width: 430px;
  aspect-ratio: 1;
  border: 1px solid #80e1d322;
  border-radius: 50%;
  box-shadow:
    0 0 0 45px #80e1d20a,
    0 0 0 90px #80e1d208;
}
.section-kicker-light {
  color: var(--mint);
}
.section-kicker-light::before {
  background: var(--mint);
  box-shadow: 0 0 0 5px #80e1d31c;
}
.statement-content {
  position: relative;
}
.statement-content h2 {
  margin: 20px 0 10px;
  font-size: clamp(34px, 5vw, 55px);
  font-weight: 900;
  line-height: 1.55;
}
.statement-content h2 span {
  color: var(--mint);
}
.statement-content p {
  margin: 0 0 27px;
  color: #c1d0d7;
  font-size: 12px;
}
.statement-watermark {
  position: absolute;
  z-index: -1;
  left: 7%;
  bottom: -150px;
  color: #ffffff08;
  font-size: 420px;
  font-weight: 900;
  line-height: 1;
}
.doctors-section {
  background: #fff;
}
.center-heading {
  margin-bottom: 43px;
  text-align: center;
}
.center-heading .section-title {
  margin-top: 12px;
  font-size: clamp(30px, 4vw, 42px);
}
.center-heading .section-description {
  margin: 8px 0 0;
}
.doctors-grid {
  display: grid;
  grid-template-columns: repeat(3, minmax(0, 1fr));
  gap: 23px;
}
.doctor-card {
  overflow: hidden;
  padding: 9px;
  border: 1px solid #e7efee;
  border-radius: 23px;
  background: white;
  box-shadow: 0 10px 32px #102c4008;
  transition:
    transform 0.35s,
    box-shadow 0.35s;
}
.doctor-card:hover {
  transform: translateY(-7px);
  box-shadow: 0 23px 48px #102c4016;
}
.doctor-photo {
  position: relative;
  height: 300px;
  overflow: hidden;
  border-radius: 17px;
  background: #eaf0ef;
}
.doctor-photo::after {
  content: '';
  position: absolute;
  inset: 45% 0 0;
  background: linear-gradient(transparent, #102c4020);
}
.doctor-photo img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  transition: transform 0.7s;
}
.doctor-card:hover .doctor-photo img {
  transform: scale(1.05);
}
.doctor-monogram {
  position: absolute;
  z-index: 1;
  left: 13px;
  top: 13px;
  width: 38px;
  height: 38px;
  display: grid;
  place-items: center;
  border: 1px solid #ffffff9c;
  border-radius: 12px;
  color: white;
  background: #102c4075;
  font-size: 11px;
  backdrop-filter: blur(8px);
}
.doctor-info {
  padding: 18px 14px 13px;
}
.doctor-label {
  color: var(--teal);
  font:
    800 8px Arial,
    sans-serif;
  letter-spacing: 1.2px;
}
.doctor-info h3 {
  margin: 8px 0 4px;
  color: var(--navy);
  font-size: 17px;
  font-weight: 900;
}
.doctor-info p {
  margin: 0;
  color: #8a9aa3;
  font-size: 11px;
}
.doctor-book {
  width: 100%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-top: 17px;
  padding: 13px 14px;
  border: 1px solid #dcece9;
  border-radius: 12px;
  color: var(--teal-dark);
  background: #f4faf8;
  font-size: 11px;
  font-weight: 900;
  transition:
    background 0.2s,
    color 0.2s;
}
.doctor-book:hover {
  color: white;
  background: var(--teal-dark);
}
.booking-section {
  position: relative;
  isolation: isolate;
  overflow: hidden;
  padding: 105px 0;
  background:
    radial-gradient(circle at 8% 18%, #0c9b8e35, transparent 28%),
    radial-gradient(circle at 92% 88%, #80e1d31c, transparent 30%), var(--navy);
}
.booking-decoration {
  position: absolute;
  z-index: -1;
  width: 430px;
  aspect-ratio: 1;
  border: 1px solid #80e1d31c;
  border-radius: 50%;
  pointer-events: none;
}
.booking-decoration-one {
  top: -300px;
  right: -120px;
  box-shadow:
    0 0 0 35px #80e1d308,
    0 0 0 75px #80e1d306;
}
.booking-decoration-two {
  bottom: -330px;
  left: -150px;
}
.booking-heading {
  margin-bottom: 38px;
  color: white;
  text-align: center;
}
.booking-heading h2 {
  margin: 13px 0 8px;
  font-size: clamp(31px, 4vw, 45px);
  font-weight: 900;
}
.booking-heading p {
  margin: 0;
  color: #b9c9d1;
  font-size: 12px;
}
.booking-panel {
  position: relative;
  max-width: 920px;
  margin: auto;
  padding: 42px 48px;
  border: 1px solid #ffffffb5;
  border-radius: 25px;
  background: white;
  box-shadow: 0 30px 90px #0003;
}
.form-intro {
  display: flex;
  align-items: center;
  gap: 16px;
  margin-bottom: 31px;
  padding-bottom: 24px;
  border-bottom: 1px solid #edf1f1;
}
.form-step {
  display: flex;
  align-items: center;
  gap: 7px;
  color: var(--teal);
  font:
    800 11px Arial,
    sans-serif;
}
.form-step i {
  width: 27px;
  height: 1px;
  background: #c8e5e0;
}
.form-intro h3 {
  margin: 0;
  color: var(--navy);
  font-size: 18px;
  font-weight: 900;
}
.form-intro p {
  margin: 4px 0 0;
  color: #9aa8ae;
  font-size: 10px;
}
.form-section-title {
  display: flex;
  align-items: center;
  gap: 10px;
  margin: 26px 0 16px;
  color: var(--navy);
  font-size: 13px;
  font-weight: 900;
}
.form-section-title > span {
  width: 25px;
  height: 25px;
  display: grid;
  place-items: center;
  border-radius: 8px;
  color: var(--teal-dark);
  background: #e9f7f4;
  font-size: 10px;
}
.form-grid {
  display: grid;
  grid-template-columns: repeat(2, minmax(0, 1fr));
  gap: 18px 20px;
}
.field {
  display: grid;
  gap: 9px;
  color: #304958;
  font-size: 11px;
  font-weight: 800;
}
.field > span b {
  color: #d66b61;
}
.field input,
.field select {
  width: 100%;
  min-height: 52px;
  padding: 0 15px;
  border: 1px solid #e1e9e9;
  border-radius: 12px;
  outline: none;
  color: var(--ink);
  background: #f9fbfb;
  font-size: 12px;
  transition:
    border 0.2s,
    box-shadow 0.2s,
    background 0.2s;
}
.field input:focus,
.field select:focus {
  border-color: var(--teal);
  background: white;
  box-shadow: 0 0 0 4px #0c9b8e13;
}
.field input::placeholder {
  color: #aab6bb;
}
.field small {
  margin-top: -4px;
  color: #9aa8ae;
  font-size: 9px;
  font-weight: 400;
}
.form-footer {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  margin-top: 30px;
  padding-top: 23px;
  border-top: 1px solid #edf1f1;
}
.form-footer p {
  max-width: 420px;
  margin: 0;
  color: #84959d;
  font-size: 10px;
  line-height: 2;
}
.form-footer p span {
  margin-left: 5px;
  color: var(--teal);
}
.submit-button {
  flex: 0 0 auto;
}
.success-state {
  max-width: 620px;
  margin: auto;
  padding: 30px 0;
  text-align: center;
}
.success-icon {
  width: 68px;
  height: 68px;
  display: grid;
  place-items: center;
  margin: 0 auto 22px;
  border-radius: 22px;
  color: var(--teal-dark);
  background: #e8f8f4;
  font-size: 31px;
}
.success-state h3 {
  margin: 13px 0;
  color: var(--navy);
  font-size: 23px;
  font-weight: 900;
}
.success-state p {
  margin: 0 auto 25px;
  color: #778991;
  font-size: 12px;
  line-height: 2.4;
}
.success-state p strong {
  color: var(--navy);
}
.contact-section {
  background: #f8fbfa;
}
.contact-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  align-items: center;
  gap: 80px;
}
.contact-copy .section-description {
  max-width: 450px;
  margin-top: 13px;
}
.contact-list {
  display: grid;
  gap: 22px;
  margin-top: 33px;
}
.contact-row {
  display: flex;
  align-items: center;
  gap: 14px;
}
.contact-icon {
  width: 48px;
  height: 48px;
  display: grid;
  flex: 0 0 48px;
  place-items: center;
  border: 1px solid #d5eee9;
  border-radius: 15px;
  color: var(--teal-dark);
  background: #eaf7f4;
  font-size: 21px;
}
.contact-row div {
  display: grid;
  gap: 5px;
}
.contact-row small {
  color: #92a1a8;
  font-size: 10px;
}
.contact-row strong {
  color: var(--navy);
  font-size: 12px;
}
.contact-visual {
  position: relative;
  min-height: 410px;
  overflow: hidden;
  border: 8px solid white;
  border-radius: 25px;
  box-shadow: var(--shadow);
  background: #e5efed;
}
.contact-visual > img {
  position: absolute;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
}
.contact-image-caption {
  position: absolute;
  right: 18px;
  bottom: 18px;
  display: flex;
  align-items: center;
  gap: 11px;
  padding: 12px 15px;
  border: 1px solid #ffffff75;
  border-radius: 13px;
  color: white;
  background: #102c40b5;
  backdrop-filter: blur(12px);
}
.contact-image-caption > span {
  color: var(--mint);
  font-size: 22px;
}
.contact-image-caption div {
  display: grid;
  gap: 4px;
}
.contact-image-caption strong {
  font:
    800 10px Arial,
    sans-serif;
  letter-spacing: 1.4px;
}
.contact-image-caption small {
  color: #c4d4d9;
  font:
    9px Arial,
    sans-serif;
}
.site-footer {
  padding: 65px 0 22px;
  color: white;
  background: #0b2436;
}
.footer-main {
  display: grid;
  grid-template-columns: 1.4fr 0.8fr 0.8fr;
  gap: 65px;
  padding-bottom: 42px;
}
.brand-footer .brand-copy strong {
  color: white;
}
.footer-brand p {
  max-width: 280px;
  margin: 18px 0 0;
  color: #9eb0ba;
  font-size: 11px;
  line-height: 2.2;
}
.footer-main h3 {
  margin: 4px 0 19px;
  color: white;
  font-size: 12px;
}
.footer-links {
  display: grid;
  align-content: start;
  gap: 12px;
}
.footer-links a,
.footer-contact p {
  margin: 0;
  color: #9eb0ba;
  font-size: 11px;
  transition: color 0.2s;
}
.footer-links a:hover,
.footer-contact a:hover {
  color: var(--mint);
}
.footer-contact p + p {
  margin-top: 10px;
}
.footer-contact a {
  display: inline-flex;
  gap: 10px;
  margin-top: 18px;
  color: var(--mint);
  font-size: 11px;
  font-weight: 800;
}
.footer-bottom {
  display: flex;
  justify-content: space-between;
  gap: 20px;
  padding-top: 20px;
  border-top: 1px solid #ffffff18;
  color: #748b98;
  font-size: 9px;
}
.footer-bottom a {
  color: #9eb0ba;
}
.drop-enter-active,
.drop-leave-active {
  transition:
    opacity 0.2s,
    transform 0.2s;
}
.drop-enter-from,
.drop-leave-to {
  opacity: 0;
  transform: translateY(-8px);
}
@keyframes float {
  0%,
  100% {
    transform: translateY(0) rotate(-2deg);
  }
  50% {
    transform: translateY(-12px) rotate(-1deg);
  }
}
@keyframes fade-up {
  from {
    opacity: 0;
    transform: translateY(25px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
@keyframes hero-drift {
  from {
    transform: scale(1.025);
  }
  to {
    transform: scale(1.09);
  }
}

@media (max-width: 1000px) {
  .desktop-nav {
    gap: 17px;
    font-size: 11px;
  }
  .hero-grid {
    gap: 35px;
  }
  .about-grid {
    gap: 55px;
  }
  .contact-grid {
    gap: 45px;
  }
}
@media (max-width: 800px) {
  .site-header {
    padding: 12px 14px 0;
  }
  .nav-wrap {
    height: 68px;
    padding-inline: 15px;
    border-radius: 16px;
  }
  .desktop-nav,
  .nav-cta {
    display: none;
  }
  .menu-toggle {
    display: block;
  }
  .mobile-nav {
    display: grid;
    gap: 4px;
    width: min(500px, 100%);
    margin: 9px auto 0;
    padding: 13px;
    border: 1px solid #ffffff;
    border-radius: 17px;
    background: #fffffff2;
    box-shadow: 0 15px 35px #102c4020;
    backdrop-filter: blur(18px);
  }
  .mobile-nav a {
    padding: 11px 12px;
    border-radius: 9px;
    color: #506674;
    font-size: 12px;
    font-weight: 700;
  }
  .mobile-nav a:hover {
    color: var(--teal-dark);
    background: #edf8f6;
  }
  .mobile-nav .mobile-booking {
    color: white;
    background: var(--teal-dark);
  }
  .hero {
    min-height: auto;
    padding: 145px 0 80px;
  }
  .hero-grid {
    grid-template-columns: 1fr;
    gap: 45px;
  }
  .hero-copy {
    max-width: 650px;
  }
  .hero-feature {
    width: min(390px, 90%);
    justify-self: start;
    margin-right: 5%;
  }
  .hero-bottom {
    display: none;
  }
  .trust-grid {
    grid-template-columns: repeat(2, 1fr);
    row-gap: 22px;
  }
  .trust-item:nth-child(2) {
    border-left: 0;
  }
  .section {
    padding: 82px 0;
  }
  .about-grid {
    grid-template-columns: 1fr;
    gap: 55px;
  }
  .about-visual {
    width: min(560px, 95%);
    margin-inline: auto;
  }
  .about-main-image {
    height: 420px;
  }
  .about-copy {
    max-width: 650px;
  }
  .services-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .doctors-grid {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }
  .doctor-card:last-child {
    grid-column: 1/-1;
    width: calc(50% - 12px);
    justify-self: center;
  }
  .contact-grid {
    grid-template-columns: 1fr;
    gap: 45px;
  }
  .contact-visual {
    min-height: 350px;
  }
  .footer-main {
    gap: 35px;
  }
}
@media (max-width: 560px) {
  .container {
    width: min(100% - 34px, 1160px);
  }
  .brand-mark {
    width: 40px;
    height: 40px;
  }
  .brand-copy strong {
    font-size: 18px;
  }
  .hero {
    padding-top: 130px;
    padding-bottom: 65px;
  }
  .hero h1 {
    margin-top: 22px;
    font-size: clamp(43px, 12vw, 60px);
    letter-spacing: -1.5px;
  }
  .hero-copy > p {
    font-size: 12px;
  }
  .hero-actions {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 9px;
  }
  .hero-actions .button {
    min-height: 51px;
    padding-inline: 10px;
    font-size: 11px;
  }
  .hero-feature {
    width: 88%;
    margin-right: 3%;
  }
  .hero-feature-image {
    height: 210px;
  }
  .trust-strip {
    padding: 24px 0;
  }
  .trust-grid {
    gap: 15px 8px;
  }
  .trust-item {
    gap: 9px;
    padding: 7px 8px;
  }
  .trust-item strong {
    font-size: 11px;
  }
  .trust-item small {
    font-size: 9px;
  }
  .section {
    padding: 68px 0;
  }
  .about-main-image {
    height: 330px;
  }
  .about-note {
    left: -5px;
    padding: 12px;
  }
  .about-note strong {
    font-size: 10px;
  }
  .about-note small {
    font-size: 8px;
  }
  .about-side-label {
    display: none;
  }
  .section-title {
    font-size: 32px;
  }
  .section-heading {
    display: block;
    margin-bottom: 28px;
  }
  .section-heading .section-description {
    margin-top: 10px;
  }
  .services-grid {
    grid-template-columns: 1fr;
    gap: 16px;
  }
  .service-image {
    height: 215px;
  }
  .service-content p {
    min-height: 0;
  }
  .statement-section {
    padding: 75px 0;
  }
  .statement-content h2 {
    font-size: 36px;
  }
  .doctors-grid {
    grid-template-columns: 1fr;
    gap: 17px;
  }
  .doctor-card:last-child {
    grid-column: auto;
    width: 100%;
  }
  .doctor-photo {
    height: 310px;
  }
  .booking-section {
    padding: 72px 0;
  }
  .booking-heading {
    margin-bottom: 27px;
  }
  .booking-heading p {
    max-width: 300px;
    margin-inline: auto;
    line-height: 2;
  }
  .booking-panel {
    padding: 25px 18px;
    border-radius: 20px;
  }
  .form-grid {
    grid-template-columns: 1fr;
    gap: 15px;
  }
  .form-footer {
    align-items: stretch;
    flex-direction: column;
  }
  .form-footer p {
    max-width: none;
  }
  .submit-button {
    width: 100%;
  }
  .contact-visual {
    min-height: 280px;
  }
  .footer-main {
    grid-template-columns: 1fr 1fr;
    gap: 30px 20px;
  }
  .footer-brand {
    grid-column: 1/-1;
  }
  .footer-bottom {
    flex-direction: column;
    align-items: center;
    text-align: center;
  }
}
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    scroll-behavior: auto !important;
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}

/* White + light-blue visual direction: no navy surfaces */
:root {
  --navy: #397fb7;
  --navy-deep: #eaf5ff;
  --teal: #68afe8;
  --teal-dark: #397fb7;
  --mint: #b8ddfa;
  --ink: #315d7d;
  --muted: #7896ad;
  --paper: #f8fcff;
  --line: #e3f0fa;
  --shadow: 0 22px 65px rgba(74, 142, 194, 0.13);
}
.site-shell,
body {
  background: #f8fcff;
  color: #315d7d;
}
.site-header {
  position: absolute;
}
.nav-wrap {
  border-color: #e4f1fb;
  background: rgba(255, 255, 255, 0.96);
  box-shadow: 0 12px 38px rgba(94, 157, 205, 0.13);
}
.brand-mark {
  background: linear-gradient(145deg, #8dc9f2, #4b9bd5);
  box-shadow: 0 8px 22px #8dc9f240;
}
.brand-copy strong,
.section-title,
.form-intro h3,
.form-section-title,
.success-state h3,
.success-state p strong,
.contact-row strong {
  color: #397fb7;
}
.brand-copy strong span,
.section-title span {
  color: #80bce8;
}
.button-primary {
  background: linear-gradient(135deg, #8bc8f2, #4e9bd7);
  box-shadow: 0 10px 25px #6aafe833;
}
.button-primary:hover:not(:disabled) {
  box-shadow: 0 16px 32px #6aafe844;
}
.desktop-nav a::after {
  background: #78b9e8;
}
.desktop-nav a:hover,
.section-kicker,
.section-title span {
  color: #4b98d1;
}
.hero {
  min-height: 760px;
  color: #315d7d;
  background: linear-gradient(120deg, #f5fbff 0%, #e8f5ff 54%, #fafdff 100%);
}
.hero-image {
  opacity: 0.08;
  filter: saturate(0.45);
}
.hero-shade {
  background: linear-gradient(270deg, rgba(248, 252, 255, 0.96), rgba(235, 247, 255, 0.88));
}
.hero-orb-one {
  background: #8cc9f455;
}
.hero-orb-two {
  background: #c4e5fc88;
}
.eyebrow {
  border-color: #d7ebfa;
  color: #4b83ad;
}
.eyebrow-dark {
  background: #ffffffc9;
}
.live-dot {
  background: #6db5e9;
  box-shadow: 0 0 0 5px #8cc9f433;
}
.hero h1 {
  color: #397fb7;
}
.hero h1 em {
  color: #82bce8;
}
.hero-copy > p,
.hero-proof span {
  color: #7192aa;
}
.button-glass {
  color: #4c83aa;
  border-color: #cfe5f6;
  background: #ffffffb8;
}
.button-glass:hover {
  color: #397fb7;
  background: white;
}
.proof-avatars img {
  border-color: #f3faff;
}
.hero-feature {
  width: min(455px, 100%);
  padding: 15px;
  border-color: #dceefa;
  background: rgba(255, 255, 255, 0.88);
  box-shadow: 0 28px 70px rgba(86, 151, 200, 0.18);
}
.hero-feature-image {
  height: 330px;
  border-radius: 18px;
}
.hero-feature-bottom strong {
  color: #397fb7;
  font-size: 15px;
}
.hero-feature-bottom small {
  color: #6aa9d8;
}
.feature-spark {
  color: white;
  background: #7dbbe8;
}
.image-chip {
  color: #397fb7;
  border-color: #d7eafa;
  background: #ffffffdf;
}
.image-chip i {
  background: #78b9e8;
}
.hero-bottom {
  color: #82a4bd;
}
.scroll-line {
  background: #b7d9f0;
}
.trust-strip,
.about-section,
.contact-section {
  background: #fff;
}
.trust-number,
.section-kicker::before,
.note-icon,
.contact-icon {
  color: #4c98d2;
  background-color: #e8f5ff;
}
.trust-number {
  background: transparent;
}
.about-outline {
  border-color: #b9ddf5;
}
.about-note {
  box-shadow: 0 15px 40px #75b3df20;
}
.service-card,
.doctor-card {
  border-color: #e1eff9;
  box-shadow: 0 9px 28px #75b3df12;
}
.service-card:hover,
.doctor-card:hover {
  border-color: #b9dcf4;
  box-shadow: 0 24px 50px #75b3df25;
}
.statement-section {
  color: #397fb7;
  background: #eaf6ff;
}
.statement-bg {
  background:
    linear-gradient(90deg, rgba(235, 247, 255, 0.94), rgba(248, 252, 255, 0.91)),
    url('https://images.unsplash.com/photo-1629909613654-28e377c37b09?w=1800&q=85') center/cover;
}
.statement-section .section-kicker,
.statement-section .statement-content h2 {
  color: #397fb7;
}
.statement-section .statement-content p {
  color: #7192aa;
}
.booking-section {
  background: linear-gradient(135deg, #eaf6ff, #dcefff 55%, #f5fbff);
}
.booking-heading {
  color: #397fb7;
}
.booking-heading p {
  color: #7192aa;
}
.booking-panel {
  border-color: #d9ebf8;
  box-shadow: 0 30px 90px #6da9d326;
}
.form-section-title > span,
.success-icon {
  color: #4b98d1;
  background: #e8f5ff;
}
.field input:focus,
.field select:focus {
  border-color: #78b9e8;
  box-shadow: 0 0 0 4px #78b9e81c;
}
.contact-visual {
  background: #eaf5fc;
}
.contact-image-caption {
  color: #397fb7;
  border-color: #d8ebf8;
  background: #ffffffdf;
}
.contact-image-caption > span {
  color: #6daee0;
}
.contact-image-caption small {
  color: #7192aa;
}
.site-footer {
  color: #397fb7;
  background: #eaf5fc;
}
.brand-footer .brand-copy strong,
.footer-main h3 {
  color: #397fb7;
}
.footer-brand p,
.footer-links a,
.footer-contact p,
.footer-bottom,
.footer-bottom a {
  color: #7192aa;
}
.footer-links a:hover,
.footer-contact a:hover {
  color: #397fb7;
}
.footer-contact a {
  color: #4b98d1;
}
.footer-bottom {
  border-color: #d5e9f7;
}
.mobile-nav {
  border-color: #e0effa;
  box-shadow: 0 15px 35px #75b3df20;
}
.mobile-nav .mobile-booking {
  background: #69afe4;
}
@media (max-width: 800px) {
  .hero-feature {
    width: min(455px, 96%);
    margin-inline: auto;
    justify-self: center;
  }
}
@media (max-width: 560px) {
  .hero-feature {
    width: 96%;
    margin-inline: auto;
  }
  .hero-feature-image {
    height: 270px;
  }
}
</style>
