<!--
  - coding=utf-8
  - Copyright 2021-present, the Recognai S.L. team.
  -
  - Licensed under the Apache License, Version 2.0 (the "License");
  - you may not use this file except in compliance with the License.
  - You may obtain a copy of the License at
  -
  -     http://www.apache.org/licenses/LICENSE-2.0
  -
  - Unless required by applicable law or agreed to in writing, software
  - distributed under the License is distributed on an "AS IS" BASIS,
  - WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
  - See the License for the specific language governing permissions and
  - limitations under the License.
  -->

<template>
  <div class="breadcrumbs">
    <ul>
      <li v-for="breadcrumb in filteredBreadcrumbs" :key="breadcrumb.name">
        <NuxtLink
          class="breadcrumbs__item"
          v-if="breadcrumb.link"
          :to="breadcrumb.link"
          >{{ breadcrumb.name }}
        </NuxtLink>
        <span
          class="breadcrumbs__item --action"
          v-else
          @click="$emit('breadcrumb-action', breadcrumb.action)"
          >{{ breadcrumb.name }}</span
        >
      </li>
    </ul>
    <re-action-tooltip tooltip="Copied">
      <a
        v-if="copyButton"
        class="breadcrumbs__copy"
        href="#"
        @click.prevent="
          $copyToClipboard(
            filteredBreadcrumbs[filteredBreadcrumbs.length - 1].name
          )
        "
      >
        <svgicon name="copy" width="16" height="16" />
      </a>
    </re-action-tooltip>
  </div>
</template>

<script>
export default {
  props: {
    breadcrumbs: {
      type: Array,
      required: true,
    },
    copyButton: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    filteredBreadcrumbs() {
      return this.breadcrumbs.filter((breadcrumb) => breadcrumb.name);
    },
  },
};
</script>

<style lang="scss" scoped>
.breadcrumbs {
  margin-right: auto;
  margin-left: 1em;
  display: flex;
  align-items: center;
  ul {
    display: flex;
    padding-left: 0;
    font-weight: normal;
    list-style: none;
  }
  li {
    margin: auto 0.5em auto auto;
    white-space: nowrap;
    &:not(:last-child):after {
      content: "/";
      margin-left: 0.5em;
    }
    &:last-child {
      word-break: break-all;
      white-space: pre-line;
      font-weight: 600;
      a {
        cursor: default;
        pointer-events: none;
      }
    }
  }
  &__copy {
    &:hover {
      .svg-icon {
        fill: darken($lighter-color, 10%);
      }
    }
    .svg-icon {
      fill: $lighter-color;
    }
  }
  &__item {
    color: $lighter-color;
    text-decoration: none;
    outline: none;
    &.--action {
      cursor: pointer;
    }
  }
}
</style>
