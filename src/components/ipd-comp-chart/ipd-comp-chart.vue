<template>
  <section class="ipd-comp-chart">
    <div class="ipd-comp-chart__desktop">
      <div class="ipd-comp-chart__header">
        <div class="ipd-comp-chart__header-spacer"></div>
        <div
          v-for="item in products"
          :key="item.id"
          class="ipd-comp-chart__header-product"
          :class="
            item.type ? `ipd-comp-chart__header-product--${item.type}` : ''
          "
        >
          {{ item.name }}
        </div>
      </div>
      <div class="ipd-comp-chart__table">
        <div
          class="ipd-comp-chart__table-feature"
          v-for="item in firstFeatures"
          :key="item.id"
        >
          <div class="ipd-comp-chart__table-text">
            {{ item.text }}<sup>{{ item.legal }}</sup>
          </div>
          <div class="ipd-comp-chart__table-product">
            <span alt="" v-if="item.online.checkmark" role="presentation">
              <svg
                height="11"
                viewBox="0 0 12 11"
                width="12"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="m8.93333333 15.1247438c-.192.2146273-.38533333.2146273-.57733333 0l-.34533333-.3875215-4.01066667-4.1271037 1.616-1.99871653 2.94266667 3.06291013 5.71066663-6.6743122 1.7306667 1.87053636z"
                  fill="#2ca01c"
                  fill-rule="evenodd"
                  transform="translate(-4 -5)"
                />
              </svg>
            </span>
            <span v-if="item.online.tip">{{ item.online.tip }}</span>
          </div>
          <div class="ipd-comp-chart__table-product">
            <span alt="" role="presentation" v-if="item.desktop.checkmark">
              <svg
                height="11"
                viewBox="0 0 12 11"
                width="12"
                xmlns="http://www.w3.org/2000/svg"
              >
                <path
                  d="m8.93333333 15.1247438c-.192.2146273-.38533333.2146273-.57733333 0l-.34533333-.3875215-4.01066667-4.1271037 1.616-1.99871653 2.94266667 3.06291013 5.71066663-6.6743122 1.7306667 1.87053636z"
                  fill="#393A3D"
                  fill-rule="evenodd"
                  transform="translate(-4 -5)"
                />
              </svg>
            </span>

            <span v-if="item.desktop.tip">{{ item.desktop.tip }}</span>
          </div>
        </div>
        <div
          ref="expandingContent"
          class="ipd-comp-chart__table-expandable"
          :style="{ height: expandedScrollHeight }"
        >
          <div
            class="ipd-comp-chart__table-feature"
            v-for="item in secondFeatures"
            :key="item.id"
          >
            <div class="ipd-comp-chart__table-text">
              {{ item.text }}<sup>{{ item.legal }}</sup>
            </div>
            <div class="ipd-comp-chart__table-product">
              <span alt="" v-if="item.online.checkmark" role="presentation">
                <svg
                  height="11"
                  viewBox="0 0 12 11"
                  width="12"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m8.93333333 15.1247438c-.192.2146273-.38533333.2146273-.57733333 0l-.34533333-.3875215-4.01066667-4.1271037 1.616-1.99871653 2.94266667 3.06291013 5.71066663-6.6743122 1.7306667 1.87053636z"
                    fill="#2ca01c"
                    fill-rule="evenodd"
                    transform="translate(-4 -5)"
                  />
                </svg>
              </span>
              <span v-if="item.online.tip">{{ item.online.tip }}</span>
            </div>
            <div class="ipd-comp-chart__table-product">
              <span alt="" role="presentation" v-if="item.desktop.checkmark">
                <svg
                  height="11"
                  viewBox="0 0 12 11"
                  width="12"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    d="m8.93333333 15.1247438c-.192.2146273-.38533333.2146273-.57733333 0l-.34533333-.3875215-4.01066667-4.1271037 1.616-1.99871653 2.94266667 3.06291013 5.71066663-6.6743122 1.7306667 1.87053636z"
                    fill="#393A3D"
                    fill-rule="evenodd"
                    transform="translate(-4 -5)"
                  />
                </svg>
              </span>

              <span v-if="item.desktop.tip">{{ item.desktop.tip }}</span>
            </div>
          </div>
        </div>
      </div>
      <button
        v-if="expandable"
        class="ipd-comp-chart__more"
        @click="toggleChart()"
      >
        <span v-if="expanded">Show less</span>
        <span v-else>Show more</span>
      </button>
    </div>
    <div class="ipd-comp-chart__mobile" ref="mobileDisplay">
      <div class="ipd-comp-chart__selector-container">
        <div class="ipd-comp-chart__selector-wrapper">
          <div
            class="ipd-comp-chart__selector-magicline"
            ref="magicLine"
            :style="{ left: `${magicLeft}px`, right: `${magicRight}px` }"
            :class="
              magicDirection === 'right'
                ? 'ipd-comp-chart__selector-magicline--right'
                : ''
            "
          ></div>
          <ul class="ipd-comp-chart__selector">
            <li
              class="ipd-comp-chart__selector-item"
              v-for="(product, index) in mobileProductsList"
              :key="product.id"
            >
              <button
                class="ipd-comp-chart__selector-button"
                :class="
                  activeCompChartId === product.id
                    ? `ipd-comp-chart__selector-button--active`
                    : ''
                "
                @click="(event) => shiftCompChart(event, index, product.id)"
                :id="`${id}__tab-contorl__${index}`"
                :ref="`${product.id}`"
              >
                {{ product.name }}
              </button>
            </li>
          </ul>
        </div>
      </div>
      <div class="ipd-comp-chart__columns-wrapper">
        <div
          class="ipd-comp-chart__columns"
          v-touch:swipe="(direction) => swipeHandler(direction)"
        >
          <ul
            class="ipd-comp-chart__column"
            :class="
              activeCompChartId === product.id
                ? 'ipd-comp-chart__column--active'
                : ''
            "
            v-for="product in mobileProductsList"
            :key="product.id"
            :style="{ transform: transformTranslateShift }"
          >
            <li
              class="ipd-comp-chart__column-item"
              v-for="feature in product.features"
              :key="feature.id"
              :aria-describedby="
                feature.desktop.tip !== null && product.type === 'desktop'
                  ? `tooltip-${product.id}-${feature.id}`
                  : false
              "
              @click.stop="toggleToolTip(`${product.id}-${feature.id}`)"
              @keyup.enter.stop="toggleToolTip(`${product.id}-${feature.id}`)"
              v-on-clickaway="hideToolTip"
            >
              <transition name="tooltip">
                <div
                  role="tooltip"
                  :id="`tooltip-${product.id}-${feature.id}`"
                  v-if="
                    toolTip === `${product.id}-${feature.id}` &&
                    feature.desktop.tip !== null &&
                    product.type === 'desktop'
                  "
                  class="ipd-comp-chart__column-item-tip"
                >
                  <div class="ipd-comp-chart__column-item-tip-text">
                    {{ feature.desktop.tip }}
                  </div>
                </div>
              </transition>
              <span class="ipd-comp-chart__column-item-icon">
                <svg
                  v-if="feature[product.type].tip"
                  class="ipd-comp-chart__column-item-info"
                  width="24px"
                  height="24px"
                  viewBox="0 0 24 24"
                  version="1.1"
                  xmlns="http://www.w3.org/2000/svg"
                  xmlns:xlink="http://www.w3.org/1999/xlink"
                >
                  <path
                    d="M12,2 C17.514,2 22,6.486 22,12 C22,17.514 17.514,22 12,22 C6.486,22 2,17.514 2,12 C2,6.486 6.486,2 12,2 Z M12,20 C16.411,20 20,16.411 20,12 C20,7.589 16.411,4 12,4 C7.589,4 4,7.589 4,12 C4,16.411 7.589,20 12,20 Z M12,11 C11.448,11 11,11.447 11,12 L11,15 C11,15.553 11.448,16 12,16 C12.552,16 13,15.553 13,15 L13,12 C13,11.447 12.552,11 12,11 M12,8 C11.448,8 11,8.447 11,9 C11,9.553 11.448,10 12,10 C12.552,10 13,9.553 13,9 C13,8.447 12.552,8 12,8"
                    id="path-1"
                  ></path>
                </svg>
                <svg
                  v-if="feature[product.type].checkmark"
                  class="ipd-comp-chart__column-item-check"
                  data-v-52499c8f=""
                  height="11"
                  viewBox="0 0 12 11"
                  width="12"
                  xmlns="http://www.w3.org/2000/svg"
                >
                  <path
                    data-v-52499c8f=""
                    d="m8.93333333 15.1247438c-.192.2146273-.38533333.2146273-.57733333 0l-.34533333-.3875215-4.01066667-4.1271037 1.616-1.99871653 2.94266667 3.06291013 5.71066663-6.6743122 1.7306667 1.87053636z"
                    fill="#2ca01c"
                    fill-rule="evenodd"
                    transform="translate(-4 -5)"
                  ></path>
                </svg>
              </span>
              <span
                class="ipd-comp-chart__column-text"
                :class="
                  !feature[product.type].tip && !feature[product.type].checkmark
                    ? 'ipd-comp-chart__column-text--fade'
                    : ''
                "
              >
                {{ feature.text }}
              </span>
            </li>
          </ul>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
const products = [
  {
    id: "qbo",
    name: "QuickBooks Online",
    type: "online",
  },
  {
    id: "qbdt",
    name: "QuickBooks Desktop",
    type: "desktop",
  },
];
const firstFeatures = [
  {
    id: "feat1",
    text: "Create professional invoices",
    legal: null,
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: true,
      tip: null,
    },
  },
  {
    id: "feat2",
    text: "Track sales and expenses",
    legal: null,
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: true,
      tip: null,
    },
  },
  {
    id: "feat3",
    text: "Create reports with 1 click",
    legal: null,
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: true,
      tip: null,
    },
  },
  {
    id: "feat4",
    text: "Schedule automatic invoices",
    legal: null,
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: null,
    },
  },
  {
    id: "feat5",
    text: "Work from any device—PC, Mac, tablet, or phone",
    legal: "1",
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: null,
    },
  },
];

const secondFeatures = [
  {
    id: "feat6",
    text: "Connect to hundreds of online apps",
    legal: "2",
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: null,
    },
  },
  {
    id: "feat7",
    text: "Automatically download and organize bank transactions",
    legal: "3",
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: "Manual process",
    },
  },
  {
    id: "feat8",
    text: "Phone support",
    legal: "4",
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: "Starts at $47/month",
    },
  },
  {
    id: "feat9",
    text: "Get the latest updates automatically",
    legal: null,
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: "$299 upgrade or Desktop subscription",
    },
  },
  {
    id: "feat10",
    text: "Access it online (online hosting)",
    legal: "5",
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: "$40/month and up per user",
    },
  },
  {
    id: "feat11",
    text: "Access for up to 5 users",
    legal: "6",
    online: {
      checkmark: true,
      tip: null,
    },
    desktop: {
      checkmark: false,
      tip: "3-user license starts at $61/month",
    },
  },
  {
    id: "feat12",
    text: "Track inventory and set reorder points",
    legal: null,
    online: {
      checkmark: false,
      tip: null,
    },
    desktop: {
      checkmark: true,
      tip: null,
    },
  },
  {
    id: "feat13",
    text: "Calculate and rebill job costs",
    legal: null,
    online: {
      checkmark: false,
      tip: null,
    },
    desktop: {
      checkmark: true,
      tip: null,
    },
  },
  {
    id: "feat14",
    text: "Calculate discounts by customer",
    legal: null,
    online: {
      checkmark: false,
      tip: null,
    },
    desktop: {
      checkmark: true,
      tip: null,
    },
  },
];

import { directive as onClickaway } from "vue-clickaway2";

export default {
  name: "IPDCompChart",
  props: ["expandable"],
  directives: {
    onClickaway: onClickaway,
  },
  data() {
    return {
      expanded: false,
      products: products,
      firstFeatures: firstFeatures,
      secondFeatures: secondFeatures,
      activeCompChart: 0,
      activeCompChartId: null,
      mobileVisible: null,
      magicDirection: null,
      magicLeft: 100,
      magicRight: 100,
      prevIndex: 0,
      id: null,
      toolTip: null,
    };
  },
  created() {
    this.$set(this.$data, "id", this._uid);
  },
  mounted() {
    this.$set(this.$data, "activeCompChartId", "qbo");
    this.checkMobileVisibility();
    this.setMagicLinePos(this.prevIndex);
    window.addEventListener("resize", this.checkMobileVisibility);
    window.addEventListener("resize", this.resizeSetMagicLinePos);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.checkMobileVisibility);
    window.removeEventListener("resize", this.resizeSetMagicLinePos);
  },
  methods: {
    toggleToolTip: function (id) {
      if (this.toolTip === id) {
        this.$set(this.$data, "toolTip", null);
      } else {
        this.$set(this.$data, "toolTip", id);
      }
    },
    hideToolTip: function () {
      this.$set(this.$data, "toolTip", null);
    },
    checkMobileVisibility: function () {
      let visibility = getComputedStyle(
        this.$refs.mobileDisplay
      ).getPropertyValue("display");

      visibility !== "none"
        ? this.$set(this.$data, "mobileVisible", true)
        : this.$set(this.$data, "mobileVisible", false);
    },
    toggleChart: function () {
      this.$set(this.$data, "expanded", !this.$data.expanded);
    },
    swipeHandler: function (direction) {
      if (direction === "right" && this.activeCompChart !== 0) {
        let index = this.activeCompChart - 1;
        this.$set(this.$data, "activeCompChart", index);
        this.$set(this.$data, "activeCompChartId", this.products[index].id);
        this.setMagicLinePos(index);
      } else if (
        direction === "left" &&
        this.activeCompChart !== this.products.length - 1
      ) {
        let index = this.activeCompChart + 1;
        this.$set(this.$data, "activeCompChart", index);
        this.$set(this.$data, "activeCompChartId", this.products[index].id);
        this.setMagicLinePos(index);
      }
    },
    shiftCompChart: function (event, index, id) {
      this.$set(this.$data, "activeCompChart", index);
      this.$set(this.$data, "activeCompChartId", id);
      this.setMagicLinePos(index, event.target);
    },
    setMagicLinePos: function (index, elem) {
      if (this.mobileVisible) {
        let target = elem
            ? elem
            : document.getElementById(`${this.id}__tab-contorl__${index}`),
          left = target.offsetLeft,
          width = target.offsetWidth,
          parentWidth = target.offsetParent.offsetWidth,
          right = parentWidth - (left + width),
          direction = index < this.prevIndex ? "left" : "right";

        this.$set(this.$data, "magicDirection", direction);
        this.$set(this.$data, "magicLeft", left);
        this.$set(this.$data, "magicRight", right);
        this.$set(this.$data, "prevIndex", index);
      }
    },
    resizeSetMagicLinePos: function () {
      this.setMagicLinePos(this.activeCompChart);
    },
  },
  computed: {
    transformTranslateShift: function () {
      return `translate3d(calc(-${this.activeCompChart * 85}%),0,0)`;
    },
    expandedScrollHeight: function () {
      if (this.$data.expanded === true || this.expandable === false) {
        return `${this.$refs.expandingContent.scrollHeight}px`;
      } else {
        return 0;
      }
    },
    mobileProductsList: function () {
      let prods = [],
        feats = [...this.firstFeatures, ...this.secondFeatures];

      this.products.map((item) => {
        prods.push(item);
      });

      prods.map((item) => {
        item["features"] = feats;
      });

      return prods;
    },
  },
  watch: {
    toolTip: function () {
      if (!this.toolTip) {
        window.removeEventListener("keyup", this.hideToolTip);
      } else {
        window.addEventListener("keyup", this.hideToolTip);
      }
    },
  },
};
</script>

<style lang="scss" scoped>
@import "./ipd-comp-chart.scss";
</style>
