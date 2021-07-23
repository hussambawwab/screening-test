<template>
  <div class="profile">
    <header></header>
    <div class="content">
      <div class="content-1">
        <div class="content-1-1">
          <div class="content-1-1-1">
            <div class="content-1-1-1-1">
              <div class="row">
                <div class="col-6">
                  <div class="input-section">
                    <i class="fa fa-search icon-search"></i>

                    <input
                      v-model="search"
                      type="text"
                      placeholder="Find customers"
                      class="input-filed"
                    />
                  </div>
                </div>
                <div class="col-6">
                  <div class="select-section">
                    <select v-model="sortSelection">
                      <option value="" disabled selected hidden>
                        Filter by Name (A-Z)
                      </option>
                      <option value="1">Name (A-Z)</option>
                      <option value="2">Name (Z-A)</option>
                    </select>
                  </div>
                </div>
              </div>

              <div class="profiles-section">
                <div
                  class="profile-card"
                  v-for="profile in filteredList"
                  :key="profile.id"
                >
                  <div class="profile-card-1">
                    <div class="profile-card-1-1">
                      <div class="profile-contant">
                        <div class="profile-img-section">
                          <div class="action-buttons">
                            <div class="buttons-section">
                              <button class="edit-button">Edit</button>
                              <button class="view-button">View</button>
                            </div>
                          </div>
                          <div class="profile-img-section-1">
                            <div class="profile-img-section-1-1">
                              <img
                                class="profile-img"
                                src="https://images-ext-1.discordapp.net/external/bFSBwdEXrFOXuZjh71Y8zbudgD2Tl585Em4UHhYWgW0/https/upload.wikimedia.org/wikipedia/commons/7/7c/Profile_avatar_placeholder_large.png"
                              />
                            </div>
                          </div>
                        </div>
                        <div class="profile-text-section">
                          <div class="user-name">{{ profile.name }}</div>

                          <div class="user-sub-name">
                            @{{ profile.username }}
                          </div>
                          <div class="user-description">
                            "{{ profile.company.catchPhrase }}"
                          </div>
                          <div class="user-info">
                            <i
                              class="fa fa-envelope-o user-info-email-icon"
                            ></i>
                            <div class="user-info-text">
                              {{ profile.email }}
                            </div>
                          </div>
                          <div class="user-info">
                            <i
                              class="fa fa-map-marker user-info-location-icon"
                            ></i>

                            <div class="user-info-text">
                              {{ profile.address.street }} ,
                              {{ profile.address.suite }} ,
                              {{ profile.address.city }} ,
                              {{ profile.address.zipcode }} ,
                              {{ profile.address.geo.lat }} ,
                              {{ profile.address.geo.lng }} ,
                            </div>
                          </div>
                          <div class="user-info">
                            <span class="phone-icon"></span>
                            <div class="user-info-text">
                              {{ profile.phone }}
                            </div>
                          </div>
                          <div class="user-info">
                            <span class="web-icon"></span>
                            <div class="user-info-text">
                              {{ profile.website }}
                            </div>
                          </div>
                          <div class="user-info">
                            <span class="work-bag-icon"></span>
                            <div class="user-info-text">
                              {{ profile.company.bs }}
                            </div>
                          </div>
                          <div class="user-info">
                            <span class="company-icon"></span>
                            <div class="user-info-text">
                              {{ profile.company.name }}
                            </div>
                          </div>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>

              <div class="no-data-section" v-if="filteredList.length === 0">
                No customer(s) found with the search criteria.
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Profiles",
  props: {
    msg: String,
  },
  data() {
    return {
      listOfProfiles: [],
      search: "",
      sortSelection: "",
    };
  },
  methods: {
    async getData() {
      fetch("http://jsonplaceholder.typicode.com/users", {
        headers: {
          "Content-Type": "application/json",
          Accept: "application/json",
        },
      })
        .then((response) => {
          console.log(response);
          return response.json();
        })
        .then((data) => {
          // Work with JSON data here
          console.log(data);
          this.listOfProfiles = data;
        })
        .catch((err) => {
          // Do something for an error here
          console.log("Error Reading data " + err);
        });
      // JSON responses are automatically parsed.
    },
  },

  created() {
    this.getData();
  },
  computed: {
    filteredList() {
      if (this.sortSelection === "1") {
        return this.listOfProfiles
          .filter((profile) => {
            return profile.name
              .toLowerCase()
              .includes(this.search.toLowerCase());
          })
          .sort(function (a, b) {
            return a.name === b.name ? 0 : a.name < b.name ? -1 : 1;
          });
      } else if (this.sortSelection === "2") {
        return this.listOfProfiles
          .filter((profile) => {
            return profile.name
              .toLowerCase()
              .includes(this.search.toLowerCase());
          })
          .sort(function (a, b) {
            return a.name === b.name ? 0 : a.name < b.name ? 1 : -1;
          });
      } else {
        return this.listOfProfiles.filter((profile) => {
          return profile.name.toLowerCase().includes(this.search.toLowerCase());
        });
      }
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.profile-img-section:hover .action-buttons {
  z-index: 1;
}
.action-buttons {
  position: absolute;
  width: 100%;
  top: 44%;
  text-align: center;
  z-index: 0;
}
.buttons-section {
  position: relative;
  width: 100%;
}
.edit-button {
  padding: 10px;
  margin: 6px;
  background: #ffffff;
  color: #1575bf;
  border: 0.5px solid #d2d5d8;
  border-radius: 8px;
}
.view-button {
  padding: 10px;
  margin: 6px;
  background: #1575bf;
  color: #ffffff;
  border: 0.5px solid #1575bf;
  border-radius: 8px;
}
.no-data-section {
  margin-top: 20%;
  text-align: center;
  font-size: 1.6rem;
  color: #202223;
}
.company-icon {
  background: url("../assets/Vector.svg");
  height: 20px;
  width: 20px;
  margin-inline-end: 12px;
}
.email-icon {
  background: url("../assets/email.svg");
  height: 20px;
  width: 20px;
  margin-inline-end: 12px;
}

.phone-icon {
  background: url("../assets/phone.svg");
  height: 20px;
  width: 20px;
  margin-inline-end: 12px;
}

.web-icon {
  background: url("../assets/web.svg");
  height: 20px;
  width: 20px;
  margin-inline-end: 12px;
}
.work-bag-icon {
  background: url("../assets/work-bag.svg");
  height: 20px;
  width: 20px;
  margin-inline-end: 12px;
}

.profile {
  display: flex;
  max-width: 100%;
  flex-direction: column;
  align-items: center;
}
header {
  width: 100%;
  height: 91px;
  overflow: hidden;

  background: #ffffff;
  box-shadow: 9px 6px 24px -2px rgba(0, 0, 0, 0.06);
}
.content {
  padding-right: 16px;
  padding-left: 16px;
  margin-top: 16px;
  max-width: 100%;
  width: 990px;
}
.content-1 {
  justify-content: center;
  box-sizing: border-box;
  flex-shrink: 0;
  flex-wrap: wrap;
  position: relative;
  display: flex;
  align-items: stretch;
  flex-direction: row;
}
.content-1-1 {
  box-sizing: border-box;
  margin: 8px;
  position: relative;
  min-width: 0;
  max-width: 876px;
  flex-shrink: 1;
  flex-basis: 876px;
  flex-grow: 1;
}
.content-1-1-1 {
  position: relative;
  width: 100%;
  display: flex;
}
.content-1-1-1-1 {
  position: relative;
  width: 100%;
  overflow: hidden;
}

.row {
  display: flex;
  flex-wrap: wrap;
  padding: 0 8px;
}
.col-6 {
  width: 50%;
}
.input-section {
  width: 100%;
  margin-bottom: 10px;
}
.input-section i {
  position: absolute;
}
.icon-search {
  padding: 10px;
  color: #5c5f62;
  min-width: 26px;
  text-align: center;
}
.input-filed {
  background: #ffffff;
  border-radius: 8px;
  border: none;
  padding: 10px 14px 10px 41px;
  width: -webkit-fill-available;
  outline: none;
  font-style: normal;
  font-weight: normal;
  color: #202223;
}
.select-section {
  position: relative;
  text-align: end;
}
.select-section select {
  padding: 8px 12px 8px 16px;
  border-radius: 4px;
  border: none;
  outline: none;
}
select option {
  display: block;
  padding: 15px;
}

.profiles-section {
  margin-top: 16px;
  flex-wrap: wrap;
  display: flex;
  flex-direction: row;
}
.profile-card {
  min-width: 228px;
  max-width: 292px;

  padding: 8px;
  min-height: 615px;
  overflow: hidden;
  box-sizing: border-box;
  flex-basis: 0px;
  position: relative;
  flex-grow: 1;
}

.profile-card-1 {
  position: relative;
  background: #ffffff;
  width: 100%;
  height: 100%;
  border-radius: 8px;
}
.profile-card-1-1 {
  position: relative;
  margin-bottom: 16px;
  box-sizing: border-box;
  flex-shrink: 0;
  max-width: 100%;
  flex-direction: column;
  display: block;
}
.profile-contant {
  min-height: 0;
  margin-top: -8px;
  box-sizing: border-box;
  margin-bottom: -8px;
  position: relative;
  display: flex;
  flex-direction: column;
  flex-grow: 1;
}
.profile-img-section {
  box-sizing: border-box;
  flex-shrink: 0;
  position: relative;
  display: flex;
  max-width: 100%;
  flex-direction: column;
  margin: 0px 0 8px 0;
  cursor: pointer;
}
.profile-img-section-1 {
  padding-top: 100%;
  position: relative;
  width: 100%;
  height: 0;
}

.profile-img-section-1-1 {
  min-height: 0;
  padding-right: 0;
  box-sizing: border-box;
  padding-bottom: 0;
  position: absolute;
  margin-bottom: 0;
  right: 0;
  margin-top: 0;
  top: 0;
  padding-top: 0;
  left: 0;
  display: flex;
  justify-content: space-between;
  bottom: 0;
  min-width: 0;
  margin-left: 0;
  align-items: stretch;
  flex-shrink: 1;
  margin-right: 0;
  flex-direction: column;
  flex-grow: 1;
  padding-left: 0;
}
.profile-img {
  box-sizing: border-box;
  border-radius: 8px 8px 0 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  border: 0;
}
.profile-text-section {
  box-sizing: border-box;
  flex-shrink: 0;
  position: relative;
  display: flex;
  max-width: 100%;
  flex-direction: column;
  margin: 6px 0;
  padding: 14px;
}
.user-name {
  font-style: normal;
  font-weight: 500;
  font-size: 24px;
  color: #202223;
}
.user-sub-name {
  font-style: normal;
  font-weight: normal;
  font-size: 16px;
  color: #909090;
  margin: 8px 0;
}
.user-description {
  font-style: normal;
  font-weight: normal;
  font-size: 16px;
  color: #51c5ff;
  margin: 10px 0;
}
.user-info {
  font-style: normal;
  font-weight: normal;
  font-size: 16px;
  color: #202223;
  display: flex;
  margin-bottom: 12px;
}
.user-info i {
  color: #015989;
}
.user-info-email-icon {
  font-size: 16px;
  margin-inline-end: 12px;
}
.user-info-location-icon {
  font-size: 22px;
  margin-inline-end: 15px;
}
@media only screen and (max-width: 450px) {
  .col-6 {
    width: 100%;
  }
  .content[data-v-71cec71a] {
    padding-right: 0;
    padding-left: 0;
    width: 100%;
  }
  .profile-card {
    max-width: 100%;
    min-height: 444px;
  }
  .profile-contant {
    flex-direction: row-reverse;
  }
  .profile-img-section {
    display: contents;
  }
  .profile-text-section {
    max-width: 50%;
  }
  .profile-img-section-1 {
    padding-top: 35%;
    margin: 22px;
  }
  .user-info {
    width: 262px;
  }
  .profile-img {
    border-radius: 8px;
  }
}
</style>
