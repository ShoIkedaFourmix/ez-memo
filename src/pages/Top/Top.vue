<template>
  <div class="top">
    <div class="glow-bg">
      <div class="glow"></div>
    </div>
    <loading ref="loading"></loading>
    <logo :class="{'logo_pos': positionChange}" />
    <div class="search form" :style="{'opacity': positionChange ? 1 : 0}">
      <div class="row">
        <div class="col-12">
          <input type="text"
                 placeholder="Enter keyword to search"
                 class="form-control"
                 v-model="this.$root.keyword"
                 @change="search()"
                 @keyup.enter="search()"
                 @input="search()"
          >

        </div>
      </div>
    </div>
    <scene-nav ref="sceneNav" :class="{'nav_pos': positionChange}" v-on:scene-changed="sceneChanged($event)">
      <scene-nav-link name="search">
        <template v-slot:text>
          Search
        </template>
        <template v-slot:icon>
          <icon icon="search"/>
        </template>
      </scene-nav-link>
      <scene-nav-link name="memos" @click="loadData()">
        <template v-slot:text>
          Memos
        </template>
        <template v-slot:icon>
          <icon icon="book"/>
        </template>
      </scene-nav-link>
      <scene-nav-link name="folders" v-if="this.$root.isLogin()" @click="loadFolders()">
        <template v-slot:text>
          Folders
        </template>
        <template v-slot:icon>
          <icon icon="folder"/>
        </template>
      </scene-nav-link>
      <scene-nav-link name="my_page" v-if="this.$root.isLogin()">
        <template v-slot:text>
          My Page
        </template>
        <template v-slot:icon>
          <icon icon="user"/>
        </template>
      </scene-nav-link>
      <scene-nav-link name="test1" v-if="!this.$root.isLogin()"></scene-nav-link>
      <scene-nav-link name="login" data-bs-toggle="modal" data-bs-target="#login" v-if="!this.$root.isLogin()">
        <template v-slot:text>
          Login
        </template>
        <template v-slot:icon>
          <icon icon="sign-in-alt"/>
        </template>
      </scene-nav-link>
      <scene-nav-link name="register" data-bs-toggle="modal" data-bs-target="#newUserModal" v-if="!this.$root.isLogin()">
        <template v-slot:text>
          New User
        </template>
        <template v-slot:icon>
          <icon icon="user-plus"/>
        </template>
      </scene-nav-link>
      <scene-nav-link name="settings" data-bs-toggle="modal" data-bs-target="#settingsModal" v-if="this.$root.isLogin()">
        <template v-slot:text>
          Settings
        </template>
        <template v-slot:icon>
          <icon icon="cog"/>
        </template>
      </scene-nav-link>
      <scene-nav-link name="test2" v-if="!this.$root.isLogin()"></scene-nav-link>
      <scene-nav-link name="logout" data-bs-toggle="modal" data-bs-target="#logout" v-if="this.$root.isLogin()">
        <template v-slot:text>
          Logout
        </template>
        <template v-slot:icon>
          <icon icon="sign-out-alt"/>
        </template>
      </scene-nav-link>
    </scene-nav>
    <transition name="fade">
      <div class="contents" v-if="positionChange">
        <div class="row row-cols-1 row-cols-md-3 g-4" v-if="activeScene !== 'folders'">
          <div class="col" v-for="memo in this.$root.memos" :key="memo.id">
            <div class="card">
              <div class="card-body">
                <h5 class="card-title">
                  {{ memo.attributes.title }}
                  <span class="badge bg-success" v-if="memo.attributes.is_public">Public</span>
                </h5>
                <p class="card-text">{{ memo.attributes.contents }}</p>
                <a href="#" class="btn btn-primary">
                  <div class="corner left_top"></div>
                  <div class="corner left_bottom"></div>
                  <div class="corner right_top"></div>
                  <div class="corner right_bottom"></div>
                  Detail
                </a>
              </div>
              <div class="card-footer text-muted d-flex justify-content-between">
                <span>Created by {{ memo.attributes.user_id ? 'aaa' : 'None' }}</span>
                <span>{{ memo.attributes.created_at_humans }}</span>
              </div>
            </div>
          </div>
        </div>
        <div class="row" v-else>
          <div class="col-3">
            <ul class="list-group">
              <li class="list-group-item list-group-item-action"
                  :class="{'active': this.$root.activeFolder === folder.id}"
                  v-for="folder in folders"
                  :key="folder.id"
                  @click="changeFolder(folder.id)">
                {{ folder.name }}
              </li>
            </ul>
          </div>
          <div class="col-9 p-4">
            <div class="row row-cols-1 row-cols-md-3 g-4">
              <div class="col" v-for="memo in this.$root.memos" :key="memo.id">
                <div class="card">
                  <div class="card-body">
                    <h5 class="card-title">
                      {{ memo.attributes.title }}
                      <span class="badge bg-success" v-if="memo.attributes.is_public">Public</span>
                    </h5>
                    <p class="card-text">{{ memo.attributes.contents }}</p>
                    <a href="#" class="btn btn-primary">
                      <div class="corner left_top"></div>
                      <div class="corner left_bottom"></div>
                      <div class="corner right_top"></div>
                      <div class="corner right_bottom"></div>
                      Detail
                    </a>
                  </div>
                  <div class="card-footer text-muted d-flex justify-content-between">
                    <span>Created by {{ memo.attributes.user_id ? 'aaa' : 'None' }}</span>
                    <span>{{ memo.attributes.created_at_humans }}</span>
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </transition>

    <!-- New User Modal -->
    <div class="modal fade" id="newUserModal" tabindex="-1" aria-labelledby="newUserModal" aria-hidden="true">
      <div class="modal-dialog modal-xl">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="exampleModalLabel">Create User</h5>
          </div>
          <div class="modal-body">
            <div class="alert alert-danger alert-dismissible fade show" role="alert" v-for="error in createUserErrors">
              <ul class="m-0">
                <li v-for="message in error">{{ message }}</li>
              </ul>
            </div>
            <div class="form">
              <div class="mb-3">
                <label for="name" class="form-label">Name</label>
                <input type="text" class="form-control" id="name" v-model="name">
              </div>
              <div class="mb-3">
                <label for="email" class="form-label">Email address</label>
                <input type="email" class="form-control" id="email" v-model="email">
              </div>
              <div class="mb-3">
                <label for="password" class="form-label">Password</label>
                <input type="password" class="form-control" id="password" v-model="newPassword">
              </div>
              <div class="mb-3">
                <label for="re-password" class="form-label">Re-Password</label>
                <input type="password" class="form-control" id="re-password" v-model="newRePassword">
              </div>
            </div>
          </div>
          <div class="modal-footer justify-content-center">
            <button type="button" class="btn btn-primary" @click="createUser()">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Create User
            </button>
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- Settings Modal -->
    <div class="modal fade" id="settingsModal" tabindex="-1" aria-labelledby="newUserModal" aria-hidden="true">
      <div class="modal-dialog modal-xl">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="settingsModalLabel">Settings</h5>
          </div>
          <div class="modal-body">
            <div class="form">
              <div class="mb-3">
                <label for="settings-name" class="form-label">Name</label>
                <input type="text" class="form-control" id="settings-name">
              </div>
              <div class="mb-3">
                <label for="settings-password" class="form-label">Password</label>
                <input type="password" class="form-control" id="settings-password">
              </div>
              <div class="mb-3">
                <label for="settings-new-password" class="form-label">New Password</label>
                <input type="password" class="form-control" id="settings-new-password">
              </div>
              <div class="mb-3">
                <label for="re-new-password" class="form-label">Re-Password</label>
                <input type="password" class="form-control" id="re-new-password">
              </div>
            </div>
          </div>
          <div class="modal-footer justify-content-center">
            <button type="button" class="btn btn-primary">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Save
            </button>
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- Login Modal -->
    <div class="modal fade" id="login" tabindex="-1" aria-labelledby="loginModal" aria-hidden="true">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="loginModalLabel">Login</h5>
          </div>
          <div class="modal-body">
            <div class="alert alert-danger alert-dismissible fade show" role="alert" v-if="hasErrors">
              Incorrect username or password.
            </div>
            <div class="form">
              <div class="mb-3">
                <label for="login-email" class="form-label">Email address</label>
                <input type="email" class="form-control" id="login-email" v-model="username">
              </div>
              <div class="mb-3">
                <label for="login-password" class="form-label">Password</label>
                <input type="password" class="form-control" id="login-password" v-model="password">
              </div>
            </div>
          </div>
          <div class="modal-footer justify-content-center">
            <button type="button" class="btn btn-primary" @click="login()">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Login
            </button>
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- Logout Modal -->
    <div class="modal fade" id="logout" tabindex="-1" aria-labelledby="loginModal" aria-hidden="true">
      <div class="modal-dialog modal-lg">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="logoutModalLabel">Logout</h5>
          </div>
          <div class="modal-body">
            <p class="m-0">Are you sure you want to log out and exit?</p>
          </div>
          <div class="modal-footer justify-content-center">
            <button type="button" class="btn btn-primary" @click="logout()">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Yes
            </button>
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              No
            </button>
          </div>
        </div>
      </div>
    </div>
    <!-- Memo Modal -->
    <div class="modal fade" id="createMemo" tabindex="-1" aria-labelledby="createMemoModal" aria-hidden="true">
      <div class="modal-dialog modal-xl">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="createMemoModalLabel">Create Memo</h5>
          </div>
          <div class="modal-body">
            <div class="form">
              <div class="mb-3">
                <label for="title" class="form-label">Title</label>
                <input type="text" class="form-control" id="title" v-model="memoTitle">
              </div>
              <div class="mb-3">
                <label for="contents" class="form-label">Contents</label>
                <textarea class="form-control" id="contents" rows="3" v-model="memoContents"></textarea>
              </div>
            </div>
          </div>
          <div class="modal-footer justify-content-center">
            <button type="button" class="btn btn-primary" @click="createMemo()">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Memo
            </button>
            <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">
              <div class="corner left_top"></div>
              <div class="corner left_bottom"></div>
              <div class="corner right_top"></div>
              <div class="corner right_bottom"></div>
              Close
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Logo from "../../components/Logo/Logo";
import SceneNav from "../../components/SceneNav/SceneNav";
import SceneNavLink from "@/components/SceneNav/SceneNavLink";
import Loading from "@/components/Loading/Loading";
import {Modal} from "bootstrap"
export default {
  name: "Top",
  components: {Loading, SceneNavLink, SceneNav, Logo},
  data() {
    return {
      positionChange: false,
      activeScene: "register",
      username: null,
      password: null,
      hasErrors: false,
      loginModal: null,
      logoutModal: null,
      name: null,
      email: null,
      newPassword: null,
      newRePassword: null,
      createUserErrors: {},
      memoTitle: null,
      memoContents: null,
      folders: []
    }
  },
  created() {
    setTimeout(() => {
      this.$refs.loading.hide()
    }, 1500)
  },
  methods: {
    sceneChanged(newScene) {
      this.activeScene = newScene

      if (newScene === 'memos') {
        this.$root.keyword = null
        this.$root.search();
      }

      if (newScene === 'search' || newScene === 'memos' || newScene === 'folders') {
        this.positionChange = true
      } else {
        this.positionChange = false
      }
    },
    loadData() {
      this.$root.search();
    },
    loadFolders() {
      axios.get(this.routes.folders, {
        headers: {
          'Authorization': this.$root.tokenType + ' ' + this.$root.token
        }
      })
          .then((response) => {
            this.folders = response.data
          })
          .catch((error) => {
            window.console.log(error)
          }).finally(() => {

      })
    },
    search() {
      this.$refs.sceneNav.changeScene('search')
      this.$root.search();
    },
    changeFolder(id) {
      this.$root.activeFolder = id
      this.$root.search()
    },
    createUser() {
      this.$refs.loading.show()
      axios.post(this.$root.routes.register, {
        name: this.name,
        email: this.email,
        password: this.newPassword,
        password_confirmation: this.newRePassword
      })
          .then((response) => {
            this.createUserErrors = {}
            this.$root.tokenType = 'Bearer'
            this.$root.token = response.data.accessToken
            this.$root.syncToken()
            let newUserModal = Modal.getOrCreateInstance(document.querySelector('#newUserModal'))
            newUserModal.hide()
          })
          .catch((error) => {
            if (error.response.status === 422) {
              this.createUserErrors = {}
              this.createUserErrors = error.response.data.errors
            } else {
              this.createUserErrors = {
                system: "System Error."
              }
            }
          }).finally(() => {
            this.$refs.loading.hide()
          })
    },
    login() {
      this.$refs.loading.show()
      axios.post(this.$root.routes.login, {
        grant_type: 'password',
        client_id: 2,
        client_secret: '7PYtyADSsYh5yR5Dc0r8GMSzmTMwv4qY36q7X5QO',
        username: this.username,
        password: this.password,
        scope: '',
      })
          .then((response) => {
            this.hasErrors = false
            this.$root.tokenType = response.data.token_type
            this.$root.token = response.data.access_token
            this.$root.syncToken()
            let loginModal = Modal.getOrCreateInstance(document.querySelector('#login'))
            loginModal.hide()
          })
          .catch((error) => {
            this.hasErrors = true
          }).finally(() => {
            this.$refs.loading.hide()
          })
    },
    logout() {
      this.$root.tokenType = null
      this.$root.token = null
      this.$root.syncToken()
      let logoutModal = Modal.getOrCreateInstance(document.querySelector('#logout'))
      logoutModal.hide()
      window.location.read
    },
    createMemo() {
      if (this.$root.isLogin()) {
        axios.post(this.$root.routes.createUserMemo, {
          title: this.memoTitle,
          contents: this.memoContents
        }, {
          headers: {
            'Authorization': this.$root.tokenType + ' ' + this.$root.token
          }
        })
            .then((response) => {
              this.$root.search()
              let createMemoModal = Modal.getOrCreateInstance(document.querySelector('#createMemo'))
              createMemoModal.hide()
              this.memoTitle = null
              this.memoContents = null
            })
            .catch((error) => {

            })
      } else {
        axios.post(this.$root.routes.createMemo, {
          title: this.memoTitle,
          contents: this.memoContents
        })
            .then((response) => {
              this.$root.search()
              let createMemoModal = Modal.getOrCreateInstance(document.querySelector('#createMemo'))
              createMemoModal.hide()
              this.memoTitle = null
              this.memoContents = null
            })
            .catch((error) => {

            })
      }
    }
  }
}
</script>

<style src="./Top.scss" lang="scss" scoped/>