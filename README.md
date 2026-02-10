# Методичка по выполнению модуля А (Фронтенд)
## Часть 3: Создание главной страницы (Home)

**Цель:** Реализовать главную страницу согласно ТЗ: слайдер, описание фестиваля, анонсы мероприятий, информация об организаторах, счетчик дней.

### Шаги:

1. **Установка дополнительных библиотек**
   Для слайдера установим `react-slick`:
   ```bash
   npm install react-slick slick-carousel
   ```

2. **Создание компонента HeroSlider**
   В папке `src/components/` создайте файл `HeroSlider.js`:

   ```jsx
   import React from 'react';
   import Slider from 'react-slick';
   import './HeroSlider.css';
   import 'slick-carousel/slick/slick.css';
   import 'slick-carousel/slick/slick-theme.css';

   function HeroSlider() {
     const settings = {
       dots: true,
       infinite: true,
       speed: 500,
       slidesToShow: 1,
       slidesToScroll: 1,
       autoplay: true,
       autoplaySpeed: 3000,
       pauseOnHover: true,
       arrows: true
     };

     // Массив изображений (заглушки - можно заменить на реальные)
     const slides = [
       {
         id: 1,
         title: 'Фестиваль возможностей 2026',
         description: 'Крупнейшее событие для профессионального роста',
         image: 'https://via.placeholder.com/1200x400/3498db/ffffff?text=Слайд+1'
       },
       {
         id: 2,
         title: 'Множество компетенций',
         description: 'Более 50 различных направлений для развития',
         image: 'https://via.placeholder.com/1200x400/2ecc71/ffffff?text=Слайд+2'
       },
       {
         id: 3,
         title: 'Присоединяйтесь к нам',
         description: 'Станьте частью большого сообщества',
         image: 'https://via.placeholder.com/1200x400/e74c3c/ffffff?text=Слайд+3'
       }
     ];

     return (
       <div className="hero-slider">
         <Slider {...settings}>
           {slides.map((slide) => (
             <div key={slide.id} className="slider-item">
               <div className="slider-image">
                 <img 
                   src={slide.image} 
                   alt={slide.title}
                   loading="lazy"
                 />
               </div>
               <div className="slider-content">
                 <h2>{slide.title}</h2>
                 <p>{slide.description}</p>
               </div>
             </div>
           ))}
         </Slider>
       </div>
     );
   }

   export default HeroSlider;
   ```

3. **Создание стилей для HeroSlider**
   В папке `src/components/` создайте файл `HeroSlider.css`:

   ```css
   .hero-slider {
     margin: 0 auto 3rem;
     border-radius: 10px;
     overflow: hidden;
     box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
   }

   .slider-item {
     position: relative;
     height: 400px;
   }

   .slider-image img {
     width: 100%;
     height: 400px;
     object-fit: cover;
   }

   .slider-content {
     position: absolute;
     bottom: 0;
     left: 0;
     right: 0;
     background: linear-gradient(transparent, rgba(0, 0, 0, 0.7));
     color: white;
     padding: 2rem;
     text-align: center;
   }

   .slider-content h2 {
     font-size: 2rem;
     margin-bottom: 0.5rem;
     text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
   }

   .slider-content p {
     font-size: 1.2rem;
     opacity: 0.9;
   }

   /* Стили для точек навигации */
   .slick-dots {
     bottom: 20px;
   }

   .slick-dots li button:before {
     color: white;
     font-size: 12px;
   }

   .slick-prev, .slick-next {
     z-index: 1;
   }

   .slick-prev:before, .slick-next:before {
     font-size: 30px;
   }

   @media (max-width: 768px) {
     .slider-item {
       height: 300px;
     }
     
     .slider-image img {
       height: 300px;
     }
     
     .slider-content h2 {
       font-size: 1.5rem;
     }
     
     .slider-content p {
       font-size: 1rem;
     }
   }
   ```

4. **Создание компонента CountdownTimer**
   В папке `src/components/` создайте файл `CountdownTimer.js`:

   ```jsx
   import React, { useState, useEffect } from 'react';
   import './CountdownTimer.css';

   function CountdownTimer() {
     // Устанавливаем дату фестиваля (пример: 1 июня 2026)
     const festivalDate = new Date('2026-06-01').getTime();
     
     const [timeLeft, setTimeLeft] = useState({
       days: 0,
       hours: 0,
       minutes: 0,
       seconds: 0
     });

     useEffect(() => {
       const timer = setInterval(() => {
         const now = new Date().getTime();
         const difference = festivalDate - now;

         if (difference > 0) {
           const days = Math.floor(difference / (1000 * 60 * 60 * 24));
           const hours = Math.floor((difference % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
           const minutes = Math.floor((difference % (1000 * 60 * 60)) / (1000 * 60));
           const seconds = Math.floor((difference % (1000 * 60)) / 1000);

           setTimeLeft({ days, hours, minutes, seconds });
         } else {
           clearInterval(timer);
           setTimeLeft({ days: 0, hours: 0, minutes: 0, seconds: 0 });
         }
       }, 1000);

       return () => clearInterval(timer);
     }, [festivalDate]);

     return (
       <div className="countdown-timer">
         <h3>До начала фестиваля осталось:</h3>
         <div className="timer-grid">
           <div className="timer-item">
             <span className="timer-value">{timeLeft.days}</span>
             <span className="timer-label">дней</span>
           </div>
           <div className="timer-item">
             <span className="timer-value">{timeLeft.hours}</span>
             <span className="timer-label">часов</span>
           </div>
           <div className="timer-item">
             <span className="timer-value">{timeLeft.minutes}</span>
             <span className="timer-label">минут</span>
           </div>
           <div className="timer-item">
             <span className="timer-value">{timeLeft.seconds}</span>
             <span className="timer-label">секунд</span>
           </div>
         </div>
       </div>
     );
   }

   export default CountdownTimer;
   ```

5. **Создание стилей для CountdownTimer**
   В папке `src/components/` создайте файл `CountdownTimer.css`:

   ```css
   .countdown-timer {
     background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
     color: white;
     padding: 2rem;
     border-radius: 10px;
     text-align: center;
     margin: 2rem 0;
     box-shadow: 0 5px 15px rgba(0, 0, 0, 0.2);
   }

   .countdown-timer h3 {
     margin-bottom: 1.5rem;
     font-size: 1.5rem;
   }

   .timer-grid {
     display: grid;
     grid-template-columns: repeat(4, 1fr);
     gap: 1rem;
     max-width: 600px;
     margin: 0 auto;
   }

   .timer-item {
     background-color: rgba(255, 255, 255, 0.1);
     padding: 1rem;
     border-radius: 8px;
     backdrop-filter: blur(5px);
   }

   .timer-value {
     display: block;
     font-size: 2.5rem;
     font-weight: bold;
     margin-bottom: 0.5rem;
     font-family: 'Courier New', monospace;
   }

   .timer-label {
     font-size: 0.9rem;
     opacity: 0.9;
     text-transform: uppercase;
     letter-spacing: 1px;
   }

   @media (max-width: 768px) {
     .timer-grid {
       grid-template-columns: repeat(2, 1fr);
     }
     
     .timer-value {
       font-size: 2rem;
     }
   }

   @media (max-width: 480px) {
     .timer-grid {
       grid-template-columns: 1fr;
     }
   }
   ```

6. **Обновление страницы Home**
   Обновите `src/pages/Home.js`:

   ```jsx
   import React from 'react';
   import HeroSlider from '../components/HeroSlider';
   import CountdownTimer from '../components/CountdownTimer';
   import './Home.css';

   function Home() {
     // Массив мероприятий для анонса
     const events = [
       {
         id: 1,
         title: 'Открытие фестиваля',
         date: '1 июня 2026',
         description: 'Торжественная церемония открытия с участием почетных гостей'
       },
       {
         id: 2,
         title: 'Мастер-классы от экспертов',
         date: '2-3 июня 2026',
         description: 'Практические занятия по различным компетенциям'
       },
       {
         id: 3,
         title: 'Финал соревнований',
         date: '5 июня 2026',
         description: 'Определение победителей и награждение'
       }
     ];

     // Информация об организаторах
     const organizers = [
       { name: 'Министерство образования', role: 'Генеральный организатор' },
       { name: 'Центр развития талантов', role: 'Организатор мероприятий' },
       { name: 'Технопарк "Инновация"', role: 'Технический партнер' }
     ];

     return (
       <div className="home-page">
         {/* Слайдер */}
         <HeroSlider />
         
         {/* Описание фестиваля */}
         <section className="festival-description">
           <h2>О фестивале</h2>
           <div className="description-content">
             <p>
               Региональный чемпионат «Абилимпикс» — это уникальная площадка для демонстрации 
               профессионального мастерства людей с инвалидностью и ограниченными возможностями здоровья.
             </p>
             <p>
               Наша цель — повышение престижа рабочих профессий, развитие профессионального 
               образования, а также содействие трудоустройству участников.
             </p>
             <div className="values-grid">
               <div className="value-item">
                 <h3>Профессионализм</h3>
                 <p>Высокие стандарты качества во всех компетенциях</p>
               </div>
               <div className="value-item">
                 <h3>Инклюзивность</h3>
                 <p>Равные возможности для каждого участника</p>
               </div>
               <div className="value-item">
                 <h3>Инновации</h3>
                 <p>Современные технологии и подходы</p>
               </div>
             </div>
           </div>
         </section>
         
         {/* Счетчик дней */}
         <CountdownTimer />
         
         {/* Анонс мероприятий */}
         <section className="events-preview">
           <h2>Ключевые мероприятия</h2>
           <div className="events-grid">
             {events.map((event) => (
               <div key={event.id} className="event-card">
                 <div className="event-date">{event.date}</div>
                 <h3>{event.title}</h3>
                 <p>{event.description}</p>
               </div>
             ))}
           </div>
         </section>
         
         {/* Организаторы */}
         <section className="organizers">
           <h2>Организаторы</h2>
           <div className="organizers-grid">
             {organizers.map((org, index) => (
               <div key={index} className="organizer-card">
                 <h3>{org.name}</h3>
                 <p>{org.role}</p>
               </div>
             ))}
           </div>
         </section>
       </div>
     );
   }

   export default Home;
   ```

7. **Создание стилей для Home**
   В папке `src/pages/` создайте файл `Home.css`:

   ```css
   .home-page {
     max-width: 1200px;
     margin: 0 auto;
   }

   section {
     margin-bottom: 3rem;
   }

   h2 {
     color: #2c3e50;
     margin-bottom: 1.5rem;
     font-size: 2rem;
     text-align: center;
     position: relative;
     padding-bottom: 0.5rem;
   }

   h2::after {
     content: '';
     position: absolute;
     bottom: 0;
     left: 50%;
     transform: translateX(-50%);
     width: 100px;
     height: 3px;
     background-color: #3498db;
   }

   /* Описание фестиваля */
   .festival-description p {
     font-size: 1.1rem;
     line-height: 1.8;
     margin-bottom: 1rem;
     text-align: center;
     max-width: 800px;
     margin-left: auto;
     margin-right: auto;
   }

   .values-grid {
     display: grid;
     grid-template-columns: repeat(3, 1fr);
     gap: 2rem;
     margin-top: 2rem;
   }

   .value-item {
     text-align: center;
     padding: 1.5rem;
     background-color: white;
     border-radius: 8px;
     box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
     transition: transform 0.3s;
   }

   .value-item:hover {
     transform: translateY(-5px);
   }

   .value-item h3 {
     color: #3498db;
     margin-bottom: 0.5rem;
   }

   /* Мероприятия */
   .events-grid {
     display: grid;
     grid-template-columns: repeat(3, 1fr);
     gap: 2rem;
   }

   .event-card {
     background-color: white;
     border-radius: 10px;
     padding: 1.5rem;
     box-shadow: 0 3px 15px rgba(0, 0, 0, 0.1);
     transition: all 0.3s;
   }

   .event-card:hover {
     box-shadow: 0 5px 20px rgba(0, 0, 0, 0.15);
     transform: translateY(-3px);
   }

   .event-date {
     background-color: #3498db;
     color: white;
     padding: 0.5rem 1rem;
     border-radius: 20px;
     display: inline-block;
     margin-bottom: 1rem;
     font-weight: bold;
   }

   .event-card h3 {
     color: #2c3e50;
     margin-bottom: 0.5rem;
   }

   /* Организаторы */
   .organizers-grid {
     display: grid;
     grid-template-columns: repeat(3, 1fr);
     gap: 2rem;
   }

   .organizer-card {
     text-align: center;
     padding: 2rem;
     background-color: white;
     border-radius: 10px;
     box-shadow: 0 3px 10px rgba(0, 0, 0, 0.1);
     border-top: 4px solid #3498db;
   }

   .organizer-card h3 {
     color: #2c3e50;
     margin-bottom: 0.5rem;
   }

   .organizer-card p {
     color: #7f8c8d;
     font-style: italic;
   }

   /* Адаптивность */
   @media (max-width: 992px) {
     .values-grid,
     .events-grid,
     .organizers-grid {
       grid-template-columns: repeat(2, 1fr);
     }
   }

   @media (max-width: 768px) {
     h2 {
       font-size: 1.5rem;
     }
     
     .festival-description p {
       font-size: 1rem;
     }
     
     .values-grid,
     .events-grid,
     .organizers-grid {
       grid-template-columns: 1fr;
     }
     
     .value-item,
     .event-card,
     .organizer-card {
       padding: 1rem;
     }
   }
   ```

8. **Добавление всплывающих подсказок (tooltips)**
   Создайте простой компонент Tooltip в `src/components/Tooltip.js`:

   ```jsx
   import React, { useState } from 'react';
   import './Tooltip.css';

   function Tooltip({ text, children }) {
     const [isVisible, setIsVisible] = useState(false);

     return (
       <div 
         className="tooltip-container"
         onMouseEnter={() => setIsVisible(true)}
         onMouseLeave={() => setIsVisible(false)}
       >
         {children}
         {isVisible && (
           <div className="tooltip">
             {text}
           </div>
         )}
       </div>
     );
   }

   export default Tooltip;
   ```

   И соответствующие стили в `Tooltip.css`:

   ```css
   .tooltip-container {
     position: relative;
     display: inline-block;
   }

   .tooltip {
     position: absolute;
     bottom: 100%;
     left: 50%;
     transform: translateX(-50%);
     background-color: #333;
     color: white;
     padding: 0.5rem 1rem;
     border-radius: 4px;
     font-size: 0.9rem;
     white-space: nowrap;
     z-index: 1000;
     margin-bottom: 0.5rem;
     box-shadow: 0 2px 5px rgba(0, 0, 0, 0.2);
   }

   .tooltip::after {
     content: '';
     position: absolute;
     top: 100%;
     left: 50%;
     transform: translateX(-50%);
     border-width: 5px;
     border-style: solid;
     border-color: #333 transparent transparent transparent;
   }
   ```

**Проверка:**
1. Запустите приложение и проверьте главную страницу
2. Убедитесь, что слайдер работает автоматически
3. Проверьте, что счетчик дней корректно отсчитывает время
4. Убедитесь, что все секции отображаются правильно
5. Проверьте адаптивность на разных размерах экрана
6. Наведите курсор на элементы с Tooltip (добавьте их в нужные места)
