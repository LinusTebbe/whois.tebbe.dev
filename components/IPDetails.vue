<template>
  <div>
    <div v-if="response" class="card">
      <table>
        <tr>
          <td>Type</td>
          <td>IP{{ response.ipVersion }}</td>
        </tr>
        <tr>
          <td>Network Range</td>
          <td>
            {{ response.startAddress + ' - ' + response.endAddress }}
          </td>
        </tr>
        <tr v-if="response.country">
          <td>Country</td>
          <td>{{ response.country }}</td>
        </tr>
        <tr>
          <td>NetName</td>
          <td>{{ response.name }}</td>
        </tr>
        <tr v-if="response.remarks">
          <td>Description</td>
          <td>{{ response.remarks[0].description[0] }}</td>
        </tr>
        <tr>
          <td>Address</td>
          <td class="pre">
            {{ findParam(vCardArray, 'adr')[1].label }}
          </td>
        </tr>
        <tr>
          <td>E-Mail</td>
          <td>{{ findParam(vCardArray, 'email')[3] }}</td>
        </tr>
      </table>
    </div>
    <div v-if="loading" class="spinner-body">
      <svg
        class="spinner"
        width="65px"
        height="65px"
        viewBox="0 0 66 66"
        xmlns="http://www.w3.org/2000/svg"
      >
        <circle
          class="path"
          fill="none"
          stroke-width="6"
          stroke-linecap="round"
          cx="33"
          cy="33"
          r="30"
        ></circle>
      </svg>
    </div>
  </div>
</template>

<script>
import exports from 'ip-validator'

export default {
  name: 'IPDetails',
  props: {
    ip: {
      type: String,
      default: ''
    }
  },
  data() {
    return {
      response: null,
      address: null,
      vCardArray: null,
      loading: false
    }
  },
  mounted() {
    this.refresh()
  },
  methods: {
    async refresh() {
      if (!exports.ip(this.ip)) return
      this.loading = true
      this.response = await this.fetchInformation()
      this.loading = false
      if (this.response === undefined) return
      this.vCardArray = this.getVCardArray(this.response.entities)
    },
    fetchInformation() {
      return new Promise((resolve) => {
        resolve(
          this.$axios
            .$get('https://rdap.db.ripe.net/ip/' + ('' + this.ip).trim())
            // eslint-disable-next-line no-console
            .catch((error) => console.log(error))
        )
      })
    },
    getVCardArray(arrayToSearchForAddress, keyToSearchFor = 'vcardArray') {
      let returnObject = []
      for (const prop in arrayToSearchForAddress) {
        if (arrayToSearchForAddress.hasOwnProperty(prop)) {
          const index = keyToSearchFor.indexOf(prop)
          if (typeof arrayToSearchForAddress[prop] === 'object') {
            if (index > -1) {
              returnObject = returnObject.concat(
                arrayToSearchForAddress[prop][1]
              )
            } else {
              returnObject = this.getVCardArray(
                arrayToSearchForAddress[prop],
                keyToSearchFor
              ).concat(returnObject)
            }
          }
        }
      }
      return returnObject
    },
    findParam(addressArray, keyToFind) {
      return addressArray.find((element) => element[0] === keyToFind)
    }
  }
}
</script>

<style lang="scss" scoped>
// This is just to center the spinner

.spinner-body {
  position: absolute;
  top: 57px;
  bottom: 0;
  left: 0;
  right: 0;
  display: flex;
  align-items: center;
  justify-content: center;
}

// Here is where the magic happens

$offset: 187;
$duration: 1.4s;

.spinner {
  animation: rotator $duration linear infinite;
}

@keyframes rotator {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(270deg);
  }
}

.path {
  stroke-dasharray: $offset;
  stroke: #fff;
  stroke-dashoffset: 0;
  transform-origin: center;
  animation: dash $duration ease-in-out infinite;
}

@keyframes dash {
  0% {
    stroke-dashoffset: $offset;
  }
  50% {
    stroke-dashoffset: $offset/4;
    transform: rotate(135deg);
  }
  100% {
    stroke-dashoffset: $offset;
    transform: rotate(450deg);
  }
}
</style>
