
<template>
  <li class="node-tree">
    <div class="accordion">
      <button :class="[{ 'accordion-button--active': isActive }, (level % 2) ? 'accordionbtncolor2' : 'accordionbtncolor1']" class="accordion-button" @click="myFilter">
        {{ cmdName }}<span class="btn-arrow"><fa :icon="['fas', 'angle-down']" /></span>
      </button>
      <div v-if="isActive" class="accordion-content" :class="(level % 2) ? 'accordioncolor2' : 'accordioncolor1'">
        <!-- eslint-disable vue/no-v-html -->
        <table>
          <tr>
            <th>Description</th>
            <th class="description">
              {{ node[1] }}
            </th>
          </tr>
          <tr>
            <th>Syntax</th>
            <th>{{ node[2].replace(/%prefix%/g, '>') }}</th>
          </tr>
          <tr v-if="melijnArguments.length > 0">
            <th>Arguments</th>
            <th v-html="melijnArguments" />
          </tr>
          <tr v-if="discordChannelPerms.length > 0">
            <th>Discord Channel Permissions</th>
            <th v-html="discordChannelPerms" />
          </tr>
          <tr v-if="discordPerms.length > 0">
            <th>Discord Permissions</th>
            <th v-html="discordPerms" />
          </tr>
          <tr v-if="node[7].size > 0">
            <th>Runconditions</th>
            <th>{{ node[7].join(" & ") }}</th>
          </tr>
          <tr>
            <th>Permission</th>
            <th>{{ node[8] }}</th>
          </tr>
          <tr v-if="melijnHelp.length > 0">
            <th>Extra Help</th>
            <th v-html="melijnHelp" />
          </tr>
          <tr v-if="melijnExamples.length > 0">
            <th>Examples</th>
            <th v-html="melijnExamples" />
          </tr>
        </table>
        <br>
        <ul v-if="node[9] && node[9].length">
          <node v-for="child in node[9]" :key="child[0]" :node="child" :level="level + 1" />
        </ul>
      </div>
    </div>
  </li>
</template>

<script>
import { red, blue, green, purple } from '~/constants/constants.js'

export default {
  name: 'Node',
  props: {
    node: {
      type: Array,
      required: true
    },
    level: {
      type: Number,
      required: true
    }
  },
  data () {
    return {
      isActive: false
    }
  },
  computed: {
    cmdName () {
      const name = this.node[0]
      const arr = this.node[3]
      if (arr.length > 0) {
        return name + ' | ' + arr.join(' | ')
      } else {
        return name
      }
    },
    perm () {
      if (this.node[8]) {
        return 'yes'
      } else {
        return 'no'
      }
    },
    discordPerms () {
      const perms = this.node[6]
      let total = ''
      perms.forEach((element) => {
        total += '<span class="tag '
        if (red.includes(element)) {
          total += 'is-danger'
        } else if (blue.includes(element)) {
          total += 'is-info'
        } else if (green.includes(element)) {
          total += 'is-success'
        } else if (purple.includes(element)) {
          total += 'is-purple'
        } else {
          total += 'is-light'
        }

        total += '">' + element + '</span>'
      })

      return total
    },
    discordChannelPerms () {
      const perms = this.node[5]
      let total = ''
      perms.forEach((element) => {
        total += '<span class="tag '
        if (red.includes(element)) {
          total += 'is-danger'
        } else if (blue.includes(element)) {
          total += 'is-info'
        } else if (green.includes(element)) {
          total += 'is-success'
        } else if (purple.includes(element)) {
          total += 'is-purple'
        } else {
          total += 'is-light'
        }

        total += '">' + element + '</span>'
      })

      return total
    },
    melijnArguments () {
      const arg = this.node[4]
        .replace(/\r*\n/g, '<br>')
        .replace(/%prefix%/g, '>')
        .replace(/`(.*?)`/ig, '<span class="code">$1</span>')
      return arg
    },
    melijnHelp () {
      const arg = this.node[10]
        .replace(/\r*\n/g, '<br>')
        .replace(/%prefix%/g, '>')
        .replace(/`(.*?)`/ig, '<span class="code">$1</span>')
      return arg
    },
    melijnExamples () {
      const arg = this.node[11]
        .replace(/\r*\n/g, '<br>')
        .replace(/%prefix%/g, '>')
        .replace(/`(.*?)`/ig, '<span class="code">$1</span>')
      return arg
    }
  },
  methods: {
    myFilter (event) {
      this.isActive = !this.isActive
    }
  }
}
</script>

<style lang="scss">
span.code {
  padding: 4px;
  border-radius: 8px;
  background-color: $darkest;
}
span.is-purple {
  background-color: pink;
  font-weight: bold;
}
</style>

<style lang="scss" scoped>
$color1: #252529;
$color2: #34343A;
$color3: #505056;

.btn-arrow {
  display: inline-box;
  float: right;
  transition: 0.2s ease-in-out;
}

.accordion {
  box-shadow: 0 0 5px 0 #00000021;
  border-radius: 8px;
  @media (max-width: $tablet) {
    border-radius: 6px;
  }
  @media (max-width: $phone) {
    border-radius: 4px;
  }

  .accordionbtncolor2 {
    //background-color: var(--color4)
    background-color: $color3;
  }

  .accordionbtncolor1 {
    //background-color: var(--color3)
    background-color: $color3;
  }

  .accordionbtncolor2.accordion-button--active {
    //background-color: var(--color6)
    background-color: $color3;
  }

  .accordionbtncolor1.accordion-button--active {
    //background-color: var(--color5)
    background-color: $color3;
  }

  .accordioncolor2 {
    //background-color: var(--color2)
    background-color: $color2;
  }

  .accordioncolor1 {
    //background-color: var(--color1)
    background-color: $color1;
  }

  .accordion-button {
    display: block;
    cursor: pointer;
    font-size: 18px;
    width: 100%;
    padding: 15px;
    margin: 15px 0 0 0;
    border: none;
    color: $grey-light;
    text-align: left;
    border-radius: 8px;
    @media (max-width: $tablet) {
      font-size: 16px;
      padding: 12px;
      border-radius: 6px;
      margin: 12px 0 0 0;
    }
    @media (max-width: $phone) {
      font-size: 14px;
      padding: 8px;
      margin: 8px 0 0 0;
      border-radius: 4px;
    }
  }

  .accordion-button--active {
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;
  }

  .accordion-button--active + .accordion-content {
    display: block;
    border-bottom-left-radius: 8px;
    border-bottom-right-radius: 8px;
    @media (max-width: $tablet) {
      border-bottom-left-radius: 6px;
      border-bottom-right-radius: 6px;
    }
    @media (max-width: $phone) {
      border-bottom-left-radius: 4px;
      border-bottom-right-radius: 4px;
    }
  }

  .accordion-button--active .btn-arrow {
    transform: rotate(-180deg);
  }

  // .accordion-button--active::after {
  //   content: '\25b4';
  // }

  .accordion-content {
    font-size: 18px;
    display: none;
    overflow: auto;

    padding: 15px 10px 0 15px;
    @media (max-width: $tablet) {
      padding: 15px 5px 0 10px;
    }
    @media (max-width: $phone) {
      padding: 15px 5px 0 5px;
    }

    color: $grey-lite;
    table {
      tr {
        th:first-child {
          font-weight: bold;
          border-right: 2px solid $grey;
          color: $grey-laite;
          padding: 0 6px 0 0;
        }
        th {
          color: $grey-lite;
          padding-left: 6px;
          @media (max-width: $tablet) {
            font-size: 14px;
          }
          @media (max-width: $phone) {
            font-size: 11px;
          }
        }
      }
    }

    .description {
      font-style: italic;
    }
  }
}
</style>
