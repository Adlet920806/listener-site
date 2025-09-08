# listener-site
<!DOCTYPE html>
<html lang="ru">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Час свободы для мыслей — Конфиденциальный слушатель</title>
  <link href="https://assets.calendly.com/assets/external/widget.css" rel="stylesheet">
  <script src="https://assets.calendly.com/assets/external/widget.js" type="text/javascript" async></script>
  <style>
    /* ... стили как раньше ... */
  </style>
</head>
<body>
  <!-- ... остальной код страницы ... -->

  <section id="calendar">
    <div class="container">
      <h2 style="margin-bottom:16px">📅 Онлайн-бронирование</h2>
      <div id="calendly-widget" class="calendly-inline-widget" data-url="https://calendly.com/adilet/60min?locale=ru&hide_event_type_details=1&hide_gdpr_banner=1&primary_color=1d4ed8" style="min-width:320px;height:700px;"></div>
    </div>
  </section>

  <footer>
    <!-- ... -->
  </footer>

  <script>
    // Функция выбора языка для Calendly
    function updateCalendlyLocale(lang){
      const widget = document.getElementById('calendly-widget');
      let locale = 'ru';
      if(lang==='en') locale = 'en';
      if(lang==='ar') locale = 'ar';
      widget.setAttribute('data-url', `https://calendly.com/adilet/60min?locale=${locale}&hide_event_type_details=1&hide_gdpr_banner=1&primary_color=1d4ed8`);
    }

    // Перехват переключения языка
    const langButtons = document.querySelectorAll('.lang button');
    langButtons.forEach(btn=>btn.addEventListener('click', ()=>{
      updateCalendlyLocale(btn.getAttribute('data-switch'));
    }));

    // Установить язык при загрузке
    updateCalendlyLocale(localStorage.getItem('site_lang')||'ru');
  </script>
  <script>
    // Inline Calendly render per language
    function renderCalendlyInline(){
      try{
        const lang = localStorage.getItem('site_lang')||'ru';
        const container = document.querySelector('.calendly-inline-widget');
        if(!container || !window.Calendly) return;
        container.innerHTML = '';
        Calendly.initInlineWidget({
          url: calendlyUrl(lang) + '&hide_event_type_details=1&hide_gdpr_banner=1&primary_color=1d4ed8',
          parentElement: container
        });
      }catch(e){ /* no-op */ }
    }
    document.addEventListener('DOMContentLoaded', function(){
      // initial render once Calendly script loads
      const check = setInterval(()=>{ if(window.Calendly){ clearInterval(check); renderCalendlyInline(); } }, 200);
    });
    // Re-render on language switch
    document.querySelectorAll('.lang button').forEach(btn=>{
      btn.addEventListener('click', ()=> setTimeout(renderCalendlyInline, 50));
    });
  </script>
</body>
</html>
