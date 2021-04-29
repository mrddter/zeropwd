<template>
  <div class="container">
    <div class="col col-center col-12 col-sm-10 col-lg-8 pt-3">
      <h3 class="mb-3">1. Scegli il livello di sicurezza</h3>
      <div class="btn-group" role="group" aria-label="Basic example">
        <button
          type="button"
          :class="getProfileClass('simple')"
          @click="setProfile('simple', $event)"
        >
          Semplice
        </button>
        <button
          type="button"
          :class="getProfileClass('medium')"
          @click="setProfile('medium', $event)"
        >
          Media
        </button>
        <button
          type="button"
          :class="getProfileClass('complex')"
          @click="setProfile('complex', $event)"
        >
          Complessa
        </button>
        <button
          v-if="customizable"
          type="button"
          :class="getProfileClass('custom')"
          @click="setProfile('custom', $event)"
        >
          Personalizza
        </button>
      </div>
      <div v-if="profile === 'custom'" class="custom-panel p-2 mt-3">aa</div>
    </div>

    <div class="col col-center col-12 col-sm-10 col-lg-5 pt-3">
      <h3 class="mb-3">2. Inserisci questi dati</h3>
      <div class="mb-3">
        <label for="field-master-pwd" class="form-label">Master password</label>
        <div class="input-group">
          <!-- <label for="field-master-pwd" class="input-group-text c-label"
            ><i class="fas fa-lock c-icon"></i
          ></label> -->
          <input
            id="field-master-pwd"
            v-model="master"
            type="password"
            class="form-control c-input text-center"
            placeholder="es. Rocky+Balboa(927)"
          />
        </div>
      </div>
      <!-- </div>

    <div class="col col-center col-12 col-sm-10 col-lg-5 pt-3">
      <h3 class="mb-3">3. Inserisci questi dati</h3> -->
      <div class="mb-3">
        <label for="field-website" class="form-label">Sito internet</label>
        <div class="input-group">
          <!-- <label class="input-group-text c-label" for="field-website"
            ><i class="fas fa-globe c-icon"></i
          ></label> -->
          <input
            id="field-website"
            v-model="opts.site"
            type="text"
            class="form-control c-input"
            placeholder="es. gmail.com"
          />
        </div>
      </div>
      <div class="my-3">
        <label for="field-email" class="form-label">Indirizzo email o login</label>
        <div class="input-group">
          <!-- <label class="input-group-text c-label" for="field-email"
            ><i class="fas fa-at c-icon"></i
          ></label> -->
          <input
            id="field-email"
            v-model="opts.login"
            type="text"
            class="form-control c-input"
            placeholder="es. sono.io@gmail.com"
          />
        </div>
      </div>
    </div>

    <div class="col col-center col-12 col-sm-10 col-lg-5 pt-1">
      <h3 class="mb-3">3. Recupera la password e usala per accedere</h3>

      <div class="my-3">
        <button class="my-3 btn btn-danger" @click="calculate($event)">
          Dimmela&nbsp;<i class="fas fa-key"></i>
        </button>
        <div v-if="error">
          <p class="lead" v-html="errorMessage"></p>
        </div>
        <div v-if="!!code">
          <div class="my-3">
            <label for="field-code" class="form-label">Eccola!!</label>
            <div class="input-group">
              <!-- <label class="input-group-text c-label" for="field-code"
                ><i class="fas fa-key c-icon"></i
              ></label> -->
              <input :value="code" id="field-code" type="text" class="form-control c-input" />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// import { generate_password } from '../util/password.js.bak'
import { generatePassword, createFingerprint } from 'lesspass'

export default {
  name: 'Form',

  data() {
    return {
      error: false,
      errorMessage: '',
      code: null,
      master: null,
      profile: 'medium',
      customizable: false,
      opts: {
        login: null, // 'aaaaa@gmail.com',
        site: null, // 'www.google.it',
        counter: 12,
        length: 16,
        lowercase: true,
        uppercase: true,
        digits: true,
        symbols: true,
      },
    }
  },

  methods: {
    getProfileClass: function (profile) {
      return `btn btn-primary ${profile === this.profile ? 'active' : ''}`
    },

    setProfile: function (profile, event) {
      // now we have access to the native event
      if (event) {
        event.preventDefault()
      }
      this.profile = profile
    },

    applyProfileOpts: function () {
      if (this.profile === 'simple') {
        this.opts = {
          ...this.opts,
          counter: 12,
          length: 16,
          lowercase: true,
          uppercase: true,
          digits: true,
          symbols: false,
        }
      }

      if (this.profile == null || this.profile === 'medium') {
        this.opts = {
          ...this.opts,
          counter: 12,
          length: 24,
          lowercase: true,
          uppercase: true,
          digits: true,
          symbols: false,
        }
      }

      if (this.profile === 'complex') {
        this.opts = {
          ...this.opts,
          counter: 12,
          length: 32,
          lowercase: true,
          uppercase: true,
          digits: true,
          symbols: true,
        }
      }
    },

    valid: function () {
      const { login, site } = this.opts
      const errors = []
      if (!this.master) errors.push('Master password mancante')
      if (!site) errors.push('Sito mancante')
      if (!login) errors.push('Indirizzo email o login mancante')
      return errors.length > 0 ? errors : true
    },

    calculate: function (event) {
      // now we have access to the native event
      if (event) {
        event.preventDefault()
      }

      const errors = this.valid()
      if (errors && errors.length > 0) {
        this.error = true
        this.errorMessage = errors.join('<br/>')
        return
      }

      this.error = false
      this.errorMessage = null

      this.applyProfileOpts()
      generatePassword(this.opts, this.master)
        // generatePassword(this.opts, this.opts.login, this.master, this.opts)
        .then((result) => {
          this.code = result
        })
        .catch((err) => {
          console.error(err)
          this.error = true
          this.errorMessage = `${err}`
          this.code = null
        })

      createFingerprint('ajajajaj').then((result2) => {
        console.log(result2)
      })
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@media only screen and (max-width: 576px) {
  .btn-group {
    display: block;
  }

  .btn-group .btn {
    margin: 0;
    padding: 7px 0;
    display: block;
    float: none;
    width: 100%;
    border-radius: 0;
  }
}

.col-center {
  float: none;
  margin: 0 auto;
}

.c-input {
  // max-width: 460px;
  text-align: center;
}

.c-label {
  background-color: #c52e4d;
}

.c-icon {
  color: #fff;
}

.custom-panel {
  border: 1px;
  border-style: solid;
  border-color: #eee;
  border-radius: 10px;
}
</style>