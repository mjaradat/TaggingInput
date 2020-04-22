<template>
  <div class="home">
    <p class="mb-2 mt-2">Ske. 1</p>
    <Badges :items="items1" text-key="name" />
    <p class="mb-2 mt-2">Ske. 2</p>
    <Badges :items="items11" text-key="email" />
    <p class="mb-2 mt-2">Ske. 3</p>
    <Badges :items="items2" text-key="email" />
    <p class="mb-2 mt-2">Ske. 4</p>
    <Badges :items="items3" text-key="email" />

    <p class="mb-2 mt-2"></p>
    <p class="mb-2 mt-2"></p>
    <p class="mb-2 mt-2"></p>
    <p class="mb-2 mt-2"></p>
    <p class="mb-2 mt-2"></p>
    <p class="mb-2 mt-2"></p>
    <p class="mb-2 mt-2"></p>
  </div>
</template>


<script lang="ts">
import Vue from "vue";

import Badges from "../components/sceleton/Badges.vue";

const items1 = [
  {
    id: 1,
    name: "Chantal",
    email: "cbyers0@wikia.com",
    isNew: false,
    isValid: true
  },
  {
    id: 2,
    name: "Blisse",
    email: "bmoreton1@amazon.de",
    isNew: false,
    isValid: false
  }
];
const items11 = [
  {
    id: 1,
    name: "Chantal",
    email: "cbyers0@wikia.com",
    isNew: true,
    isValid: true
  },
  {
    id: 2,
    name: "Blisse",
    email: "bmoreton1@amazon.de",
    isNew: false,
    isValid: false
  }
];
const items2 = [
  {
    id: 1,
    name: "Company Group",
    email: "ebraganza1@google.it",
    isNew: false,
    isValid: true,
    group: [
      {
        id: 1,
        name: "Dominica",
        email: "dparnaby0@ycombinator.com",
        isNew: false,
        isValid: true
      },
      {
        id: 2,
        name: "Novelia",
        email: "nwawer1@nature.com",
        isNew: false,
        isValid: false
      },
      {
        id: 3,
        name: "Harli",
        email: "hdigwood2@google.es",
        isNew: false,
        isValid: true
      },
      {
        id: 4,
        name: "Elita",
        email: "eclingan3@fema.gov",
        isNew: false,
        isValid: false
      }
    ]
  },
  {
    id: 2,
    name: "Example Group",
    email: "mkoschke2@ftc.gov",
    isNew: false,
    isValid: true,
    group: [
      {
        id: 1,
        name: "Dominica",
        email: "dparnaby0@ycombinator.com",
        isNew: false,
        isValid: true
      },
      {
        id: 2,
        name: "Novelia",
        email: "nwawer1@nature.com",
        isNew: false,
        isValid: false
      },
      {
        id: 3,
        name: "Harli",
        email: "hdigwood2@google.es",
        isNew: false,
        isValid: true
      },
      {
        id: 4,
        name: "Elita",
        email: "eclingan3@fema.gov",
        isNew: false,
        isValid: false
      }
    ]
  },
  {
    id: 3,
    name: "Pansie",
    email: "pbeves0@bbb.org",
    isNew: true,
    isValid: true
  },
  {
    id: 4,
    name: "Nanci",
    email: "nbugge3@php.net",
    isNew: true,
    isValid: true
  },
  {
    id: 5,
    name: "Myrah",
    email: "mvinick4@nps.gov",
    isNew: false,
    isValid: true
  },
  {
    id: 6,
    name: "Joline",
    email: "jponde5@sbwire.com",
    isNew: false,
    isValid: true
  },
  {
    id: 7,
    name: "Selma",
    email: "ssilwood6@indiegogo.com",
    isNew: false,
    isValid: true
  },
  {
    id: 8,
    name: "Lindi",
    email: "lquarless7@indiatimes.com",
    isNew: true,
    isValid: true
  }
];

const items3 = [
  {
    id: 1,
    name: "Dominica",
    email: "dparnaby0@ycombinator.com",
    isNew: true,
    isValid: true
  },
  {
    id: 2,
    name: "Novelia",
    email: "nwawer1@nature.com",
    isNew: true,
    isValid: false
  },
  {
    id: 3,
    name: "Harli",
    email: "hdigwood2@google.es",
    isNew: false,
    isValid: true
  },
  {
    id: 4,
    name: "Elita",
    email: "eclingan3@fema.gov",
    isNew: true,
    isValid: false
  },
  {
    id: 5,
    name: "Armand",
    email: "aganiford4@so-net.ne.jp",
    isNew: true,
    isValid: false
  },
  {
    id: 6,
    name: "Joline",
    email: "jponde5@sbwire.com",
    isNew: false,
    isValid: true
  },
  {
    id: 7,
    name: "Selma",
    email: "ssilwood6@indiegogo.com",
    isNew: false,
    isValid: false
  },
  {
    id: 8,
    name: "Lindi",
    email: "lquarless7@indiatimes.com",
    isNew: true,
    isValid: false
  },
  {
    id: 9,
    name: "Sigismundo",
    email: "sgoodlud8@google.com.au",
    isNew: true,
    isValid: false
  },
  {
    id: 10,
    name: "Justine",
    email: "jflores9@mediafire.com",
    isNew: true,
    isValid: false
  },
  {
    id: 11,
    name: "Hendrik",
    email: "hberniera@china.com.cn",
    isNew: false,
    isValid: true
  },
  {
    id: 12,
    name: "Pinchas",
    email: "petteryb@1und1.de",
    isNew: true,
    isValid: true
  },
  {
    id: 13,
    name: "Dory",
    email: "dpinarc@stumbleupon.com",
    isNew: false,
    isValid: true
  },
  {
    id: 14,
    name: "Janka",
    email: "jgalletleyd@sciencedirect.com",
    isNew: false,
    isValid: true
  },
  {
    id: 15,
    name: "Isabelle",
    email: "iarnfielde@feedburner.com",
    isNew: true,
    isValid: true
  },
  {
    id: 16,
    name: "Ethelind",
    email: "erafef@patch.com",
    isNew: true,
    isValid: false
  },
  {
    id: 17,
    name: "Domini",
    email: "dschulzeg@yahoo.co.jp",
    isNew: true,
    isValid: false
  },
  {
    id: 18,
    name: "Fina",
    email: "fblunketth@scribd.com",
    isNew: false,
    isValid: false
  },
  {
    id: 19,
    name: "Sully",
    email: "shearoni@simplemachines.org",
    isNew: false,
    isValid: false
  },
  {
    id: 20,
    name: "Sophronia",
    email: "sgreshamj@tmall.com",
    isNew: false,
    isValid: true
  },
  {
    id: 21,
    name: "Nilson",
    email: "nsperek@cafepress.com",
    isNew: true,
    isValid: true
  },
  {
    id: 22,
    name: "Alfons",
    email: "ajurgensonl@blogger.com",
    isNew: true,
    isValid: true
  },
  {
    id: 23,
    name: "Padgett",
    email: "pjahnsm@washingtonpost.com",
    isNew: false,
    isValid: false
  },
  {
    id: 24,
    name: "Randene",
    email: "rpetrelluzzin@cam.ac.uk",
    isNew: true,
    isValid: true
  },
  {
    id: 25,
    name: "Robert",
    email: "rrobiono@alibaba.com",
    isNew: true,
    isValid: false
  },
  {
    id: 26,
    name: "Augustin",
    email: "aedelmannp@vistaprint.com",
    isNew: true,
    isValid: false
  },
  {
    id: 27,
    name: "Fabian",
    email: "fcrewesq@chronoengine.com",
    isNew: false,
    isValid: false
  },
  {
    id: 28,
    name: "Rozella",
    email: "rsiddeler@ask.com",
    isNew: false,
    isValid: true
  },
  {
    id: 29,
    name: "Ive",
    email: "ilangridges@seattletimes.com",
    isNew: true,
    isValid: false
  },
  {
    id: 30,
    name: "Grissel",
    email: "ggergt@ted.com",
    isNew: true,
    isValid: false
  },
  {
    id: 31,
    name: "Avivah",
    email: "agariffu@amazon.de",
    isNew: false,
    isValid: true
  },
  {
    id: 32,
    name: "Brittani",
    email: "brosthornv@cisco.com",
    isNew: true,
    isValid: true
  },
  {
    id: 33,
    name: "Ban",
    email: "bkrugerw@biglobe.ne.jp",
    isNew: false,
    isValid: true
  },
  {
    id: 34,
    name: "Iris",
    email: "istroverx@delicious.com",
    isNew: false,
    isValid: false
  },
  {
    id: 35,
    name: "Thor",
    email: "tgoracciy@ask.com",
    isNew: true,
    isValid: true
  },
  {
    id: 36,
    name: "Jan",
    email: "jleafez@4shared.com",
    isNew: true,
    isValid: true
  },
  {
    id: 37,
    name: "Hildagarde",
    email: "hmccolm10@disqus.com",
    isNew: false,
    isValid: false
  },
  {
    id: 38,
    name: "Gay",
    email: "gbeernt11@europa.eu",
    isNew: true,
    isValid: true
  },
  {
    id: 39,
    name: "Ephrem",
    email: "eelijah12@ox.ac.uk",
    isNew: true,
    isValid: true
  },
  {
    id: 40,
    name: "Kennie",
    email: "kyetts13@rambler.ru",
    isNew: true,
    isValid: true
  },
  {
    id: 41,
    name: "Melosa",
    email: "mcollison14@github.io",
    isNew: false,
    isValid: false
  },
  {
    id: 42,
    name: "Colan",
    email: "cpringer15@examiner.com",
    isNew: false,
    isValid: true
  },
  {
    id: 43,
    name: "Nichols",
    email: "nellerby16@discuz.net",
    isNew: true,
    isValid: false
  },
  {
    id: 44,
    name: "Molli",
    email: "mgallaher17@e-recht24.de",
    isNew: false,
    isValid: true
  },
  {
    id: 45,
    name: "Fiona",
    email: "fbirkmyr18@sfgate.com",
    isNew: false,
    isValid: false
  },
  {
    id: 46,
    name: "Justen",
    email: "johallihane19@nih.gov",
    isNew: true,
    isValid: false
  },
  {
    id: 47,
    name: "Leah",
    email: "ldoggrell1a@wsj.com",
    isNew: false,
    isValid: false
  },
  {
    id: 48,
    name: "Hilarius",
    email: "heilhersen1b@utexas.edu",
    isNew: true,
    isValid: true
  },
  {
    id: 49,
    name: "Gisela",
    email: "gjedrych1c@usa.gov",
    isNew: false,
    isValid: true
  },
  {
    id: 50,
    name: "Sybil",
    email: "sboothebie1d@nymag.com",
    isNew: true,
    isValid: false
  }
];

export default Vue.extend({
  data() {
    return {
      items1,
      items11,
      items2,
      items3
    };
  },
  components: {
    Badges
  }
});
</script>
