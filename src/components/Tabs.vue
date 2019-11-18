<template>
    <div class="tab">
        <div class="tab-header">
            <ul class="tab-item" ref="nav">
                <li
                    :class="tabCls(item)"
                    v-for="(item, index) in navList"
                    :key="index"
                    @click="handleChange(index)"
                >{{item.label}}</li>
            </ul>
            <div class="active-bar-link bar-animated" :style="barStyle"></div>
        </div>
        <div class="tab-panel" ref="panels" :style="contentStyle">
            <slot></slot>
        </div>
    </div>
</template>
<script>
export default {
    name: "Tabs",
    data() {
        return {
            navList: [],
            activeKey: this.value,
            barWidth: 0,
            barOffset: 0
        };
    },
    props: {
        value: [String, Number]
    },
    provide() {
        return { TabsInstance: this };
    },
    watch: {
        activeKey() {
            this.updateStatus();
            this.updateBar();
        }
    },
    computed: {
        barStyle() {
            let style = {
                visibility: "visible",
                width: `${this.barWidth}px`
            };
            style.transform = `translate3d(${this.barOffset}px, 0px, 0px)`;
            return style;
        },
        contentStyle() {
            const index = this.getTabIndex(this.activeKey);
            return { "margin-left": index > 0 ? `-${index}00%` : 0 };
        }
    },
    mounted() {},
    methods: {
        handleChange(index) {
            this.activeKey = this.navList[index].name;
            const nav = this.navList[index];
            this.$emit("tab-click", nav.name);
        },
        tabCls(item) {
            return [
                `tab-item-title`,
                {
                    [`tab-active`]: item.name === this.activeKey
                }
            ];
        },
        getPanels() {
            const TabPanels = this.$children.filter(
                item => item.$options.name === "TabPanel"
            );
            TabPanels.sort((a, b) => {
                if (a.index && b.index) {
                    return a.index > b.index ? 1 : -1;
                }
            });
            return TabPanels;
        },
        updateNav() {
            this.navList = [];
            this.getPanels().forEach((panel, index) => {
                this.navList.push({
                    label: panel.label,
                    name: panel.name || index
                });
                if (!panel.currentName) panel.currentName = index;
                if (index === 0) {
                    if (!this.activeKey)
                        this.activeKey = panel.currentName || index;
                }
            });
            this.updateStatus();
            this.updateBar();
        },
        updateBar() {
            this.$nextTick(() => {
                const index = this.getTabIndex(this.activeKey);
                if (!this.$refs.nav) return;
                const prevTabs = this.$refs.nav.querySelectorAll(
                    ".tab-item-title"
                );
                const tab = prevTabs[index];
                this.barWidth = tab ? parseFloat(tab.offsetWidth) : 0;
                if (index > 0) {
                    let offset = 0;
                    for (let i = 0; i < index; i++) {
                        offset += prevTabs[i].offsetWidth;
                    }
                    this.barOffset = offset;
                } else {
                    this.barOffset = 0;
                }
            });
        },
        getTabIndex(name) {
            return this.navList.findIndex(nav => nav.name === name);
        },
        updateStatus() {
            const navs = this.getPanels();
            navs.forEach(
                tab => (tab.show = tab.currentName === this.activeKey)
            );
        }
    }
};
</script>
<style>
html {
    height: 100%;
    padding: 0;
    margin: 0;
}

a {
    text-decoration: none;
}

a:visited {
    color: #000;
}

.clear {
    clear: both;
}

.tab {
    -webkit-box-sizing: border-box;
    box-sizing: border-box;
    margin: 0;
    padding: 0;
    color: rgba(0, 0, 0, 0.65);
    font-size: 14px;
    font-variant: tabular-nums;
    line-height: 1.5;
    list-style: none;
    -webkit-font-feature-settings: "tnum";
    font-feature-settings: "tnum";
    position: relative;
    overflow: hidden;
    zoom: 1;
}

.tab-header {
    border-bottom: 1px solid #e8e8e8;
}

.tab-item {
    list-style: none;
    padding-inline-start: 0;
    margin-block-end: 0;
}

.tab-item > .tab-item-title {
    position: relative;
    float: left;
    padding: 10px 15px;
    text-align: center;
    font-weight: 500;
    color: #000;
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "PingFang SC",
        "Hiragino Sans GB", "Microsoft YaHei", "Helvetica Neue", Helvetica,
        Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji",
        "Segoe UI Symbol";
}

.tab-item > .tab-item-title.tab-active {
    color: #1890ff;
}

.tab-item > .tab-item-title::before {
    content: "";
    position: absolute;
    left: 0;
    bottom: 0;
    height: 2px;
    width: 0;
}

.active-bar-link {
    background-color: #1890ff;
    height: 2px;
    clear: both;
}

.active-bar-link.bar-animated {
    -webkit-transition: width 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        left 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        -webkit-transform 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
    transition: width 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        left 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        -webkit-transform 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
    transition: transform 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        width 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        left 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
    transition: transform 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        width 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        left 0.3s cubic-bezier(0.645, 0.045, 0.355, 1),
        -webkit-transform 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
}

.tab-item > .tab-item-title.tab-active > a {
    color: #1890ff;
}

.tab-panel {
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    -webkit-box-orient: horizontal;
    -webkit-box-direction: normal;
    -ms-flex-direction: row;
    flex-direction: row;
    transition: margin-left 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
    will-change: margin-left;
    width: 100%;
}

.tab-panel::before {
    display: block;
    overflow: hidden;
    content: "";
}

.tab-panel-content {
    height: 0;
    padding: 0 !important;
    opacity: 0;
    pointer-events: none;
    flex-shrink: 0;
    width: 100%;
    -webkit-transition: opacity 0.45s;
    transition: opacity 0.45s;
}

.tab-panel-content.panel-active {
    flex-shrink: 0;
    height: auto;
    width: 100%;
    opacity: 1;
    -webkit-transition: opacity 0.45s;
    transition: opacity 0.45s;
}
</style>