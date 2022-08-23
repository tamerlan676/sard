<template lang="pug">
.main
  header.header
    .header-wrapper
      img(:src="require(`~/assets/images/logo.svg`)" alt="Урологическая клиника Sard")
      .nav
       ul
        nuxt-link(:to="{ path: '/', hash:'#about'}") Об услугах
        nuxt-link(:to="{ path: '/', hash:'#simptomi'}") Заболевания
        nuxt-link(:to="{ path: '/', hash:'#docs'}") Доктора
        nuxt-link(:to="{ path: '/', hash:'#services'}") Услуги
        nuxt-link(:to="{ path: '/', hash:'#operations'}") Операции
        nuxt-link(:to="{ path: '/', hash:'#contacts'}") Контакты
      .burger(@click="openMobMenu" :class="{ activeburger: activeBurger }")
        span
      a.header-btn.button(href="#" @click="openPopup") Заказать звонок
  .hero
    .hero-wrapper
      .info
        h1.animate Помощь уролога по всей территории Северной Осетии
        p.desc.animate Обращайтесь и получайте необходимую медицинскую помощь, сохранив свое время и душевное спокойствие.
        a.button(href="#" @click="openPopup") Обратиться за помощью
      .img
        img.sm(:src="require(`~/assets/images/doc.png`)" alt="врач уролог владикавказ")
        img.big(:src="require(`~/assets/images/doc-big.png`)" alt="врач уролог владикавказ")
  section.na-domu#about
    h2.root Прием уролога на дому
    .wrapper
      img.root(:src="require(`~/assets/images/na-domu.jpg`)" alt="прием уролога")
      .text.root
        p К помощи врача уролога во Владикавказе прибегают не только мужчины, но также дети и женщины. Поскольку заболевания мочеполовой системы для многих пациентов – это сугубо интимный вопрос, вызывающий стеснение, посещение клиники для прохождения обследования может откладываться.
        p Наша клиника предоставляет возможность вызвать уролога к себе домой и получить необходимую медицинскую помощь квалифицированных специалистов.
        p За консультацией уролога на дому обращаются пациенты, которые ввиду определенных обстоятельств не могут сами посетить клинику:
        ul
          li пожилые и пенсионеры
          li беременные женщины и кормящие матери
          li люди с ограниченными физическими возможностями
          li занятые люди
    a.button.root(href="#" @click="openPopup") Обратиться за помощью
  section.cases#simptomi
    h2.root В каких случаях следует вызвать уролога на дом?
    p.desc.root К самым распространенным симптомам заболеваний в области урологии относятся такие проявления, как:
    .cases-wrapper
        .case.root(v-for="(item, id) in cases" :key="id")
          img(:src="require(`~/assets/images/med-plus.svg`)" :alt="item")
          .text {{ item }}
    a.button.root(href="#" @click="openPopup") Обратиться за помощью
  section.doctors#docs
    h2.root Врачи-урологи
    .doctors-wrapper
     .doctor.root(v-for="(doc, id) in doctors" :key="id")
        img(:src="doc.photo" :alt="doc.name")
        h3 {{ doc.name }}
        p {{ doc.info }}
        a.button(href="#" @click="openPopup") Записаться к урологу
  section.services#services
    h2.root Урологическая помощь
    .services-wrapper
      .service.root(v-for="(service, id) in services" :key="id")
       h3 {{ service.title }}
       p.desc {{ service.desc }}
       .price {{ service.price }}₽
    a.button(href="#" @click="openPopup") Заказать услугу
  section.services#operations
    h2.root Урологические операции
    p.t-desc Мы проводим операции в дневном хирургическом стационаре больницы под общей и местной анестезией.
    .services-wrapper
      .service.root(v-for="(operation, id) in operations" :key="id")
       .title-with-icon
        img( style="width: 30px" :src="require(`~/assets/images/bag.svg`)" :alt="operation.title")
        h3 {{ operation.title }}
       p.desc {{ operation.desc }}
       .price {{ operation.price }}₽
    a.button(href="#" @click="openPopup") Заказать услугу
  section.questions#contacts
    .questions-wrapper
      .left
        h2.root Остались вопросы?
        p.desc.root {{ question }}
      .right
        .phone.root +7(918) 822 05 61
        a.button.root(href="#" @click="openPopup") Оставить заявку
  footer
    img(:src="require(`~/assets/images/logo.svg`)" alt="Урологическая клиника Sard")
    p 2022г. Все права защищены.
  .mob-menu(:class="{ active: isActive }")
    ul
      li(@click="openMobMenu")
        nuxt-link(:to="{ path: '/', hash:'#about'}") Об услугах
      li(@click="openMobMenu")
        nuxt-link(:to="{ path: '/', hash:'#simptomi'}") Заболевания
      li(@click="openMobMenu")
        nuxt-link(:to="{ path: '/', hash:'#docs'}") Доктора
      li(@click="openMobMenu")
        nuxt-link(:to="{ path: '/', hash:'#services'}") Услуги
      li(@click="openMobMenu")
        nuxt-link(:to="{ path: '/', hash:'#operations'}") Операции
      li(@click="openMobMenu")
        nuxt-link(:to="{ path: '/', hash:'#contacts'}") Контакты
  .popup(:class="{ active: activePopup }")
    .popup-window
      .popup-header
        img(:src="require(`~/assets/images/close.svg`)" @click="openPopup")
      form(@submit.prevent="submit")
        .fields(v-if="notSend")
          h3 Введите данные
          .field
            input(type="text" v-model="name" placeholder="Как вас зовут?" required)
          .field
            input(type="number" v-model="phone" placeholder="Ваш номер телефона" required)
          .field
            button(type="submit") Отправить
        .message(v-if="sended")
          img(:src="require(`~/assets/images/success.svg`)")
          h3 Ваш запрос отправлен!
          p Наш менеджер уже набирает ваш номер, не убирайте далеко телефон.
</template>

<script>
import axios from 'axios'
export default {
  name: 'MainPage',
  data () {
    return {
      isActive: false,
      activeBurger: false,
      activePopup: false,
      notSend: true,
      sended: false,
      question: 'Свяжитесь с нами или отправьте заявку через форму. \n Мы ответим в течении 5 минут',
      cases: [
        'дискомфорт и затруднение при мочеиспускании \n (боль, рези, жжение)',
        'нетипичные выделения из уретры',
        'болевые ощущения в области поясницы и крестца (при наличии других клинических проявлений)',
        'редкое мочеиспускание',
        'изменение цвета, запаха, наличие крови, гноя в моче',
        'недержание или частые позывы к мочеиспусканию',
        'ощущение неполного опорожнения мочевого пузыря'
      ],
      doctors: [
        {
          name: 'Карсанов Сослан',
          photo: require('~/assets/images/foto.jpg'),
          info: 'Курс повышения квалификации "Радиационная безопасность пациентов и персонала при проведении рентгенологических исследований  в   Первом Санкт-Петербургском государственном медицинском университете им.акад.И.П.Павлова.'
        },
        {
          name: 'Карсанов Сослан',
          photo: require('~/assets/images/foto.jpg'),
          info: 'Курс повышения квалификации "Радиационная безопасность пациентов и персонала при проведении рентгенологических исследований  в   Первом Санкт-Петербургском государственном медицинском университете им.акад.И.П.Павлова.'
        },
        {
          name: 'Карсанов Сослан',
          photo: require('~/assets/images/foto.jpg'),
          info: 'Курс повышения квалификации "Радиационная безопасность пациентов и персонала при проведении рентгенологических исследований  в   Первом Санкт-Петербургском государственном медицинском университете им.акад.И.П.Павлова.'
        }
      ],
      services: [
        {
          title: 'Взятие мазка из уретры',
          desc: 'Исследование необходимо для выявления скрытых инфекций и определения мед препаратов, которые будут наиболее эффективными при лечении.',
          price: '3000'
        },
        {
          title: 'Перевязка послеоперационная чистая',
          desc: 'Проводится пациентам, которые перенесли операцию.',
          price: '3000'
        },
        {
          title: 'Взятие секрета предстательной железы',
          desc: 'Данная манипуляция проводится для выявления заболеваний простаты.',
          price: '3000'
        },
        {
          title: 'Установка уретрального катетера',
          desc: 'Показана при воспалениях и повреждениях мочевыводящих путей, при травме и кровотечении, в острой стадии цистита.',
          price: '3000'
        },
        {
          title: 'Удаление уретрального катетера',
          desc: 'Процедура показана в том случае, если исчезла необходимость в катетеризации.',
          price: '3000'
        },
        {
          title: 'Смена цистостомического дренажа',
          desc: 'Проводится в случае закупоривания мочевыводящих путей, а также если катетер установить не получается. Врач подскажет, какой уход за дренажем необходим.',
          price: '3000'
        },
        {
          title: 'Массаж предстательной железы и семенных пузырьков',
          desc: 'Применяется при простатите (воспаление предстательной железы), а также при везикулите (воспалении семенных пузырьков).',
          price: '3000'
        }
      ],
      operations: [
        {
          title: 'Обрезание крайней плоти',
          desc: 'Описание операции',
          price: '15 000'
        }
      ],
      name: '',
      phone: ''
    }
  },
  mounted () {
    const observer = new IntersectionObserver((entries) => {
      entries.forEach((entry) => {
        if (entry.isIntersecting) {
          entry.target.classList.add('active')
        }
      })
    })
    document.querySelectorAll('.root').forEach((block) => { observer.observe(block) })
  },
  methods: {
    openMobMenu () {
      this.isActive = !this.isActive
      this.activeBurger = !this.activeBurger
    },
    openPopup () {
      this.activePopup = !this.activePopup
    },
    submit () {
      const order = {
        name: this.name,
        phone: this.phone
      }
      const message = `<strong>${order.name}</strong> заказал(а) звонок \n Тел: <strong><a href="tel:+${order.phone}">${order.phone}</a></strong>`
      axios.post('https://api.telegram.org/bot5496395304:AAGmLMNyhdOGpnjbue4QjiCZI9ZOfsFsjGg/sendMessage', {
        chat_id: '-1001491584824',
        parse_mode: 'html',
        text: message
      }).then(this.notSend = false, this.sended = true)
    }
  }
}
</script>

<style lang="scss" scoped>
  header{
      width: 100%;
      background: $skyblue;
      position: sticky;
      top: 0;
      z-index: 10;
    .header-wrapper{
      display: flex;
      padding: 0 16px;
      justify-content: space-between;
      align-items: center;
      height: 76px;
        @media (min-width : 1200px) {
          height: 100px;
          width: 1120px;
          margin: 0 auto;
          padding: 0;
      }
      img{
        width: 99px;
        @media (min-width : 1200px) {
            width: 119px;
        }
      }
      .nav{
          display: none;
        @media (min-width : 992px) {
          display: block;
        }
        ul{
          display: flex;
          justify-content: space-between;
          width: 690px;
          a{
            text-decoration: none;
            color: $darkgrey;
            font-weight: 500;
            &:hover{
              color: $red
            }
          }
          a.nuxt-link-active {
             font-weight: bold;
             color: $red;
           }
        }
      }
      .burger{
        position: relative;
        width: 30px;
        height: 30px;
        @media (min-width : 992px) {
          display: none;
        }

        &:before{
          position: absolute;
          content: '';
          width: 30px;
          height: 3px;
          background: $red;
          top: 5px;
          transition: all .5s ease;
        }
        &:after{
          position: absolute;
          content: '';
          width: 30px;
          height: 3px;
          background: $red;
          top: 25px;
          transition: all .5s ease;
        }
        span{
          width: 20px;
          height: 3px;
          background: $red;
          position: absolute;
          top: 15px;
          right: 0;
        }
        &.activeburger{
          span{
            width: 20px;
            height: 3px;
            background: $red;
            position: absolute;
            top: 15px;
            right: 0;
            display: none;
          }
        &:before{
          transform: rotate(40deg);
          top: 20px
        }
        &:after{
          transform: rotate(-40deg);
          top: 20px
        }
        }
      }
      .header-btn{
        display: none;
        @media (min-width : 992px) {
          display: flex;
        }
      }
    }
  }
  .hero{
      background: $skyblue;
    .hero-wrapper{
      padding: 50px 16px 0 16px;
      @media (min-width : 992px) {
        display: flex;
        align-items: center;
        padding: 50px 32px 0 32px;
      }
      @media (min-width : 1200px) {
        width: 1120px;
        margin: 0 auto;
        padding: 0;
      }
    }
    .info{
        @media (min-width : 768px) {
            width: 500px;
        }
        @media (min-width : 1200px) {
            width: 590px;
        }
        h1{
            font-size: 24px;
            line-height: 1.2;
            margin-bottom: 20px;
            @media (min-width : 1200px) {
                font-size: 40px;
            }
        }
        .desc{
            color: $darkgrey;
            margin-bottom: 20px;
        }
    }
    .img{
        height: fit-content;
        display: flex;
        justify-content: flex-end;
        @media (min-width : 768px) {
            justify-content: center;
        }
        img{
            max-width: 100%;
            height: 100%;
        }
        .sm{
            @media (min-width : 1200px) {
                display: none;
            }
        }
        .big{
            display: none;
            @media (min-width : 1200px) {
                display: block;
            }
        }
    }
  }

  .na-domu{
        @media (min-width : 1200px) {
          width: 1120px;
          margin: 0 auto;
          padding: 120px 0;
        }
     h2{
        text-align: center;
            @media (min-width : 992px) {
              text-align: left;
            }
      }
    .wrapper{
          margin-bottom: 24px;
        @media (min-width : 992px) {
          display: flex;
          flex-direction: row-reverse;
          align-items: flex-start;
          justify-content: space-between;
        }
        img{
            margin-bottom: 16px;
            border-radius: 10px;
            @media (min-width : 992px) {
              width: 550px;
            border-radius: 20px;
            }
        }
      .text{
        @media (min-width : 992px) {
          width: 520px;
          margin-right: 20px;
        }
        color: $darkgrey;
        p{
            margin-bottom: 16px;
        }
        li{
            margin-bottom: 10px;
            position: relative;
            padding-left: 15px;
            &:before{
                position: absolute;
                content: '';
                width: 5px;
                height: 5px;
                left: 0;
                background: $darkgrey;
                border-radius: 100%;
                top: 50%
            }
        }
     }
    }
  }

  .cases{
    background: $skyblue;
    h2{
        @media (min-width : 1200px) {
        width: 565px;
        margin: 0 auto 16px;
        }
    }
    .desc{
        text-align: center;
        margin-bottom: 16px;
        @media (min-width : 1200px) {
            width: 640px;
            margin: 0 auto 24px;
          }
    }
    .cases-wrapper{
        margin-bottom: 24px;
        @media (min-width : 992px) {
            grid-template-columns: 1fr 1fr;
            grid-gap: 20px;
            display: grid;
        }
        @media (min-width : 1200px) {
            width: 1120px;
            margin: 0 auto 24px;
            grid-template-columns: 1fr 1fr 1fr;
        }
        .case{
            display: flex;
            background: #fff;
            padding: 16px;
            border-radius: 10px;
            align-items: center;
            margin-bottom: 8px;
            @media (min-width : 1200px) {
                height: 90px;
            }
            img{
                margin-right: 16px;
            }
            .text{
                white-space: pre-line;
            }
        }

    }
    a{
        margin: 0 auto;
    }
  }

  .doctors{
    .doctors-wrapper{
        @media (min-width : 768px) {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            grid-gap: 16px;
        }
        @media (min-width : 992px) {
            grid-template-columns: repeat(3, 1fr);
            grid-gap: 20px
        }
        @media (min-width : 1200px) {
            width: 1120px;
            margin: 0 auto;
        }
        .doctor{
          padding: 20px;
          background: #fff;
          border-radius: 10px;
          margin-bottom: 16px;
          @media (min-width : 768px) {
            margin-bottom: 0;
          }
          img{
            margin-bottom: 10px;
            width: 100%;
          }
          h3{
            margin-bottom: 16px;
          }
          p{
            color: $darkgrey;
            margin-bottom: 24px;
          }
          a{
            width: 100%;
          }
        }
    }
  }

  .services{
    background: $skyblue;
    .t-desc{
      text-align: center;
      margin: 0 auto 32px;
    }
    .services-wrapper{
        margin-bottom: 24px;
        @media (min-width : 992px) {
            margin: 0 auto;
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            grid-gap: 20px;
            margin-bottom: 24px;
        }
        @media (min-width : 1200px) {
            width: 1120px;
        }
      .service{
        padding: 20px;
        background: #fff;
        border-radius: 10px;
        margin-bottom: 10px;
        display: flex;
        flex-direction: column;
        justify-content: space-between;
        .title-with-icon{
          display: flex;
          align-items: center;
          margin-bottom: 8px;
          img{
            margin-right: 10px;
          }
        }
        .desc{
            margin-bottom: 8px;
        }
        .price{
            color: #2F784C;
            font-weight: 600;
            text-align: right;
            font-size: 18px;
        }
      }
    }
    a{
        margin: 0 auto;
    }
  }
  .questions{
    .questions-wrapper{
      @media (min-width : 1200px) {
        width: 1120px;
        margin: 0 auto;
        display: flex;
        justify-content: space-between;
        align-items: center;
      }
      h2{
        @media (min-width : 1200px) {
          text-align: left;
        }
      }
      .desc{
        text-align: center;
        font-weight: 500;
        white-space: pre-line;
        margin-bottom: 16px;
        @media (min-width : 1200px) {
          text-align: left;
        }
      }
      .phone{
        font-weight: 600;
        font-size: 30px;
        text-align: center;
        margin-bottom: 24px;
      }
      .button{
        margin: 0 auto;
        @media (min-width : 1200px) {
          width: 100%;
        }
      }
    }
  }
  footer{
    display: flex;
    align-items: center;
    justify-content: center;
    flex-direction: column;
    background: #183656;
    padding: 80px;
    color: #fff;
    font-size: 13px;
    text-align: center;
    img{
      width: 99px;
      @media (min-width : 1200px) {
          width: 119px;
        }
      }
  }
  .mob-menu{
    width: 70%;
    background: $skyblue;
    height: 100%;
    position: fixed;
    padding: 20px;
    top: 76px;
    right: -70%;
    transition: all .5s ease;
    ul{
      a{
        display: block;
        font-size: 18px;
        font-weight: 600;
        color: $darkgrey;
        text-decoration: none;
        margin-bottom: 24px;
        text-align: right;
      }
    }
    &.active{
      right: 0;
    }
  }
  .popup{
    position: fixed;
    width: 100%;
    height: 100%;
    background-color: rgba(41, 39, 39, 0.8);
    top: 0;
    z-index: 12;
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 16px;
    display: none;
    opacity: 0;
    transition: all .5s ease;
    .popup-window{
      width: 100%;
      padding: 20px;
      border-radius: 10px;
      background: #fff;
      display: flex;
      flex-direction: column;
      position: relative;
      align-items: center;
      justify-content: center;
      @media (min-width : 768px) {
        width: 400px;
      }
      @media (min-width : 992px) {
        padding: 32px;
        width: 450px;
      }
      .popup-header{
        width: 100%;
        text-align: right;
        margin-bottom: 10px;
        img{
          cursor: pointer
        }
      }
      form{
        width: 100%;
      }
      .fields{
        h3{
          margin-bottom: 16px;
        }
      }
      .field{
        margin-bottom: 16px;
        width: 100%;
      }
      input{
        height: 40px;
        width: 100%;
        padding-left: 8px;
        border-radius: 5px;
        border: 1px solid #c6e2ff;
        font-size: 16px;
      }
      button{
        width: 100%;
        padding: 10px;
        border: none;
        background: $red;
        color: #fff;
        font-weight: 500;
        cursor: pointer;
        border-radius: 5px;
        height: 40px;
        font-size: 16px;
      }
      .message{
        text-align: center;
        h3{
          margin-bottom: 16px;
        }
        img{
          width: 200px;
          margin-bottom: 10px;
        }
      }
    }
    &.active{
      opacity: 1;
      display: flex;
    }
  }
  .root{
    transform: scale(0);
    opacity: 0;
    transition: all .7s ease;
    &.active{
      transform: scale(1);
      opacity: 1;
    }
  }

</style>
