<template>
  <div>
    <Loading v-if="overviews.length === 0" />
    <!-- <div v-if="overviews.length === 0">Loading content</div> -->
    <div v-else>
      <div class="container">
        <table class="table is-fullwidth mt-5 mb-5">
          <thead>
            <tr class="cstm-background">
              <th class="has-text-white py-4 px-5">Project</th>
              <th class="has-text-white">Sale</th>
              <th class="has-text-white">Token price</th>
              <th class="has-text-white">ATH Price</th>
              <th class="has-text-white">ROI (ATH)</th>
              <th class="has-text-white">Current price</th>
              <th class="has-text-white">ROI (current)</th>
            </tr>
          </thead>

          <tbody class="has-text-primary">
            <tr
              v-for="overview in overviews"
              :id="overview.id"
              :key="overview.id"
              class="cstm-table-row"
            >
              <!-- Project -->
              <td class="pl-5 py-5">
                <div class="is-flex">
                  <div>
                    <div
                      class="is50-50 mr-4"
                      :style="{
                        backgroundImage: `url(${overview.sale_token.logo})`,
                      }"
                    ></div>
                  </div>
                  <div
                    class="is-flex is-flex-direction-column is-justify-content-space-between"
                  >
                    <span class="has-text-weight-bold is-size-6">
                      {{ overview.name }}
                    </span>
                    <p class="decoration-after">
                      {{ overview.sale_token.ticker }}
                    </p>
                  </div>
                </div>
              </td>

              <!-- SALE -->

              <td>
                <div class="is-flex is-align-items-center">
                  <span class="secret-header is-flex-grow-2">Sale</span>
                  <div v-if="overview.contributed">
                    <span class="has-text-weight-bold icon icontablet">
                      <img
                        :src="overview.deposit_token.logo"
                        alt=""
                        class="image image-nobox is-16x16 mr-1"
                      />
                      <span>
                        {{ overview.contributed.toFixed(0) }}
                      </span>
                      /
                      <span>{{
                        (overview.token_amount * overview.token_price).toFixed(
                          0
                        )
                      }}</span>
                    </span>

                    <br />
                    <span>
                      ({{
                        (
                          (overview.contributed /
                            overview.token_price /
                            overview.token_amount) *
                          100
                        ).toFixed(2)
                      }}%)
                    </span>
                  </div>
                  <div v-else class="has-text-weight-bold">TBA</div>
                </div>
              </td>

              <!-- Token Price -->
              <td>
                <div class="is-flex is-align-items-center">
                  <span class="secret-header is-flex-grow-2">Token price</span>
                  <img
                    :src="overview.deposit_token.logo"
                    alt=""
                    class="image is-16x16 mr-1"
                  />
                  <span class="has-text-weight-bold">
                    {{
                      overview.token_price.toLocaleString(undefined, {
                        maximumSignificantDigits: 3,
                      })
                    }}
                  </span>
                </div>
              </td>

              <!-- ATH Price -->

              <td>
                <div class="is-flex is-align-items-center">
                  <span class="secret-header is-flex-grow-2">ATH price</span>
                  <div v-if="overview.ath_price" class="icon">
                    <img
                      :src="overview.deposit_token.logo"
                      alt=""
                      class="image image-nobox is-16x16 mr-1"
                    />
                    <span class="has-text-weight-bold">
                      {{
                        overview.ath_price.toLocaleString(undefined, {
                          minimumFractionDigits: 2,
                          maximumFractionDigits: 2,
                        })
                      }}
                    </span>
                  </div>
                  <div v-else class="has-text-weight-bold">TBA</div>
                </div>
              </td>

              <!-- ROI ATH -->

              <td>
                <div class="is-flex is-align-items-center">
                  <span class="secret-header is-flex-grow-2">ATH (ROI)</span>
                  <div v-if="overview.ath_roi" style="display: inline-block">
                    <span class="has-text-weight-bold is-size-5-tablet">
                      {{
                        overview.ath_roi.toLocaleString(undefined, {
                          minimumFractionDigits: 2,
                          maximumFractionDigits: 2,
                        })
                      }}x
                    </span>
                  </div>
                  <div v-else class="has-text-weight-bold">TBA</div>
                </div>
              </td>

              <!-- Current Price -->

              <td>
                <div class="is-flex is-align-items-center">
                  <span class="secret-header is-flex-grow-2"
                    >Current Price</span
                  >
                  <div v-if="overview.last_price">
                    <span class="has-text-weight-bold icon">
                      <img
                        :src="overview.deposit_token.logo"
                        alt=""
                        class="image is-16x16 mr-1"
                      />
                      {{
                        overview.last_price.toLocaleString(undefined, {
                          minimumFractionDigits: 2,
                          maximumFractionDigits: 3,
                        })
                      }}
                    </span>
                  </div>
                  <div v-else class="has-text-weight-bold">TBA</div>
                </div>
              </td>

              <!-- ROI CURRENT -->

              <td>
                <div class="is-flex is-align-items-center">
                  <span class="secret-header is-flex-grow-2"
                    >ROI (current)</span
                  >
                  <div v-if="overview.curr_roi" style="display: inline-block">
                    <div
                      v-if="overview.curr_roi < 0.1 || overview.curr_roi < 1.0"
                    >
                      <span
                        class="has-text-weight-bold is-size-4-tablet up-padding red-color"
                      >
                        {{
                          overview.curr_roi.toLocaleString(undefined, {
                            minimumFractionDigits: 2,
                            maximumFractionDigits: 2,
                          })
                        }}x
                      </span>
                    </div>
                    <div
                      v-else-if="
                        overview.curr_roi <= 1.0 || overview.curr_roi <= 1.15
                      "
                    >
                      <span
                        class="has-text-weight-bold is-size-4-tablet up-padding orange-color"
                      >
                        {{
                          overview.curr_roi.toLocaleString(undefined, {
                            minimumFractionDigits: 2,
                            maximumFractionDigits: 2,
                          })
                        }}x
                      </span>
                    </div>
                    <div v-else>
                      <span
                        class="has-text-weight-bold is-size-4-tablet up-padding green-color"
                      >
                        {{
                          overview.curr_roi.toLocaleString(undefined, {
                            minimumFractionDigits: 2,
                            maximumFractionDigits: 2,
                          })
                        }}x
                      </span>
                    </div>
                  </div>
                  <div v-else class="has-text-weight-bold">TBA</div>
                </div>
              </td>

              <!-- <td>
                <div class="is-flex is-align-items-center">
                  <span class="secret-header is-flex-grow-2"
                    >ROI (current)</span
                  >
                  <div v-if="overview.curr_roi" style="display: inline-block">
                    <span
                      class="has-text-weight-bold is-size-4-tablet up-padding"
                    >
                      {{
                        overview.curr_roi.toLocaleString(undefined, {
                          minimumFractionDigits: 2,
                          maximumFractionDigits: 2,
                        })
                      }}x
                    </span>
                  </div>
                  <div v-else class="has-text-weight-bold">TBA</div>
                </div>
              </td> -->

              <!-- END -->
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      overviews: [],
    }
  },

  created() {
    this.getOverview()
  },
  methods: {
    async getOverview() {
      const data = axios.get(`https://api.solanium.com/solana/projects`)
      const result = await data
      result.data.forEach((overview) => {
        this.overviews.push(overview)
      })

      this.overviews.sort((a, b) =>
        a.whitelist_start > b.whitelist_start ? 1 : -1
      )
    },
  },
}
</script>

<style scoped lang="scss">
.green-color {
  color: green;
}
.cstm-button {
  display: none;
}
.red-color {
  color: red;
}

.orange-color {
  color: orange;
}
.cstm-body {
  background: white;
  &:last-child {
    border-bottom-left-radius: 20px;
    border-bottom-right-radius: 20px;
  }
}
.secret-header {
  display: none;
  opacity: 0.6;
}
.cstm-table-row {
  &:hover {
    background: rgba(227, 226, 236, 0.3);
  }
}
.up-padding {
  display: block;
  padding-bottom: 3px;
}
.table {
  border-radius: 20px;
  font-size: 15px;
  .cstm-background {
    background: #170d69;
    th {
      border: none;
      &:first-child {
        border-top-left-radius: 20px;
      }
      &:last-child {
        border-top-right-radius: 20px;
      }
    }
  }
  td,
  th {
    vertical-align: middle;
  }
}
.decoration-after {
  overflow: hidden;
  text-overflow: ellipsis;
  opacity: 0.6;
}
.is50-50 {
  height: 50px;
  width: 50px;
  background-size: contain;
  background-position: center center;
  background-repeat: no-repeat;
}
@media only screen and (max-width: 900px) {
  .cstm-background {
    display: none;
  }
  .cstm-button {
    display: unset;
  }
  .secret-header {
    display: unset;
  }
  .cstm-table-row {
    display: flex;
    flex-direction: column;
    td {
      font-size: 16px;
      padding: 7px 20px;
      &:last-child {
        padding-bottom: 20px;
        border-bottom: 2px solid rgba(grey, 0.2);
      }
      &:not(:last-child) {
        border: none;
      }
    }
    &:last-child {
      td {
        border: none;
      }
    }
  }
  .image-nobox {
    display: unset;
  }
}

@media only screen and (max-width: 900px) {
  .icon {
    justify-content: end;
  }
  .icontablet {
    margin-left: 55px;
  }
}
</style>
