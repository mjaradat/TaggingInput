<template>
  <div class="page-inner">
    <div class="card">
      <div class="card-body">
        <fieldset>
          <TaggingInput
            ref="tagging"
            :isMultiSelect="true"
            placeholder="Input a color name"
            :fetch="getData"
            text-key="email"
            id-key="id"
            fieldLable="Tagging Input 1: Multiple Select"
          />
          <TaggingInput
            ref="tagging"
            :isMultiSelect="false"
            placeholder="Input a color name"
            :fetch="getData"
            text-key="email"
            id-key="id"
            fieldLable="Tagging Input 2: Single Select"
          />
        </fieldset>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import Vue from "vue";
import TaggingInput from "../components/TaggingInput.vue";
const data = [
  {
    id: 1,
    name: "Dominica",
    email: "@dparnaby0@ycombinator.com",
    isNew: false,
    isValid: true
  },
  {
    id: 2,
    name: "Novelia",
    email: "naawer1@nature.com",
    isNew: false,
    isValid: true,
    group: [
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
            isValid: true
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
            isValid: true
          }
        ]
      },
      {
        id: 1,
        name: "Company Group",
        email: "ebraganza1@google.it",
        isNew: false,
        isValid: true
      },
      {
        id: 3,
        name: "Pansie",
        email: "pbeves0@bbb.org",
        isNew: false,
        isValid: true
      }
    ]
  },
  {
    id: 3,
    name: "Harli",
    email: "nahdigwood2na@google.es",
    isNew: false,
    isValid: true
  },
  {
    id: 4,
    name: "Elita",
    email: "eclingan3@fema.gov",
    isNew: false,
    isValid: true
  },
  {
    id: 5,
    name: "Armand",
    email: "aganiford4@so-net.ne.jp",
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
    isNew: false,
    isValid: true
  },
  {
    id: 9,
    name: "Sigismundo",
    email: "sgoodlud8@google.com.au",
    isNew: false,
    isValid: true
  },
  {
    id: 10,
    name: "Justine",
    email: "jflores9@mediafire.com",
    isNew: false,
    isValid: true
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
    isNew: false,
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
    isNew: false,
    isValid: true
  },
  {
    id: 16,
    name: "Ethelind",
    email: "erafef@patch.com",
    isNew: false,
    isValid: true
  },
  {
    id: 17,
    name: "Domini",
    email: "dschulzeg@yahoo.co.jp",
    isNew: false,
    isValid: true
  },
  {
    id: 18,
    name: "Fina",
    email: "fblunketth@scribd.com",
    isNew: false,
    isValid: true
  },
  {
    id: 19,
    name: "Sully",
    email: "shearoni@simplemachines.org",
    isNew: false,
    isValid: true
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
    isNew: false,
    isValid: true
  },
  {
    id: 22,
    name: "Alfons",
    email: "ajurgensonl@blogger.com",
    isNew: false,
    isValid: true
  },
  {
    id: 23,
    name: "Padgett",
    email: "pjahnsm@washingtonpost.com",
    isNew: false,
    isValid: true
  },
  {
    id: 24,
    name: "Randene",
    email: "rpetrelluzzin@cam.ac.uk",
    isNew: false,
    isValid: true
  },
  {
    id: 25,
    name: "Robert",
    email: "rrobiono@alibaba.com",
    isNew: false,
    isValid: true
  },
  {
    id: 26,
    name: "Augustin",
    email: "aedelmannp@vistaprint.com",
    isNew: false,
    isValid: true
  },
  {
    id: 27,
    name: "Fabian",
    email: "fcrewesq@chronoengine.com",
    isNew: false,
    isValid: true
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
    isNew: false,
    isValid: true
  },
  {
    id: 30,
    name: "Grissel",
    email: "ggergt@ted.com",
    isNew: false,
    isValid: true
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
    isNew: false,
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
    isValid: true
  },
  {
    id: 35,
    name: "Thor",
    email: "tgoracciy@ask.com",
    isNew: false,
    isValid: true
  },
  {
    id: 36,
    name: "Jan",
    email: "jleafez@4shared.com",
    isNew: false,
    isValid: true
  },
  {
    id: 37,
    name: "Hildagarde",
    email: "hmccolm10@disqus.com",
    isNew: false,
    isValid: true
  },
  {
    id: 38,
    name: "Gay",
    email: "gbeernt11@europa.eu",
    isNew: false,
    isValid: true
  },
  {
    id: 39,
    name: "Ephrem",
    email: "eelijah12@ox.ac.uk",
    isNew: false,
    isValid: true
  },
  {
    id: 40,
    name: "Kennie",
    email: "kyetts13@rambler.ru",
    isNew: false,
    isValid: true
  },
  {
    id: 41,
    name: "Melosa",
    email: "mcollison14@github.io",
    isNew: false,
    isValid: true
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
    isNew: false,
    isValid: true
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
    isValid: true
  },
  {
    id: 46,
    name: "Justen",
    email: "johallihane19@nih.gov",
    isNew: false,
    isValid: true
  },
  {
    id: 47,
    name: "Leah",
    email: "ldoggrell1a@wsj.com",
    isNew: false,
    isValid: true
  },
  {
    id: 48,
    name: "Hilarius",
    email: "heilhersen1b@utexas.edu",
    isNew: false,
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
    isNew: false,
    isValid: true
  }
];
export default Vue.extend({
  methods: {
    getData(query: string, textKey: string): Promise<Array<{}>> {
      return new Promise<Array<any>>((resolve, reject) => {
        const items = data;
        setTimeout(function() {
          if (!query) {
            reject();
          }
          if (query === "@showall") {
            resolve(items);
            return;
          }
          const filteredItems = items.filter(item => {
            const tempItem = Object.create(item);
            return tempItem[textKey].startsWith(query);
          });
          resolve(filteredItems);
        }, Math.floor(Math.random() * Math.floor(5)) * 1000);
      });
    }
  },
  components: {
    TaggingInput
  },
  mounted() {
    console.log(this.$refs.tagging);
  }
});
</script>