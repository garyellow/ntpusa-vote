<template>
  <div class="flex flex-wrap justify-center">
    <ElSteps
      direction="vertical"
      align-center
      space="12dvh"
      class="mt-5 w-full !flex-wrap content-center"
    >
      <ElStep
        v-for="(activity, index) in activities"
        :key="index"
        :status="style(activity.start, activity.end)"
        class="tracking-[1.5px]"
      >
        <template #title>
          <div class="font-bold sm:text-lg">
            {{ activity.content }}
          </div>
        </template>
        <template #description>
          <div class="min-w-max sm:text-base">
            <span>{{
              activity.start.toLocaleString(undefined, {
                dateStyle: "long",
                timeStyle: activity.showTime ? "medium" : undefined,
              })
            }}</span>
            <span v-if="activity.showEnd"> 〜 </span>
            <span v-if="activity.showEnd && !activity.showTime">{{
              activity.end.toLocaleString(undefined, {
                dateStyle: "long",
                timeStyle: activity.showTime ? "medium" : undefined,
              })
            }}</span>
          </div>
          <div
            v-if="activity.showEnd && activity.showTime"
            class="min-w-max sm:text-base"
          >
            {{
              activity.end.toLocaleString(undefined, {
                dateStyle: "long",
                timeStyle: activity.showTime ? "medium" : undefined,
              })
            }}
          </div>
        </template>
      </ElStep>
    </ElSteps>
    <ElButton
      v-if="status === 'authenticated'"
      type="success"
      class="z-10 -mt-5 mb-8"
      @click="useRouter().push('/vote')"
    >
      <span class="font-bold">前 往 投 票 頁 面</span>
    </ElButton>
    <ElButton
      v-else
      type="primary"
      class="z-10 -mt-5 mb-8"
      @click="useRouter().push('/login')"
    >
      <span class="font-bold">前 往 登 入 頁 面</span>
    </ElButton>
  </div>
</template>

<script setup lang="ts">
definePageMeta({
  auth: false,
  title: "首頁",
});

const { status } = useAuthState();

const style = (start: Date, end: Date) => {
  return Date.now() < start.getTime()
    ? "process"
    : Date.now() < end.getTime()
      ? "finish"
      : "success";
};

const activities = [
  {
    content: "登記參選",
    start: new Date(2024, 3, 9, 22, 0, 0),
    end: new Date(2024, 3, 12, 23, 59, 59),
    showEnd: true,
    showTime: true,
  },
  {
    content: "資格審定及補件",
    start: new Date(2024, 3, 13),
    end: new Date(2024, 3, 14, 23, 59, 59),
    showEnd: true,
    showTime: false,
  },
  {
    content: "公告名單",
    start: new Date(2024, 3, 15),
    end: new Date(2024, 3, 16),
    showEnd: false,
    showTime: false,
  },
  {
    content: "公告選舉公報",
    start: new Date(2024, 3, 16),
    end: new Date(2024, 3, 17),
    showEnd: false,
    showTime: false,
  },
  {
    content: "候選人政見發表會",
    start: new Date(2024, 4, 4),
    end: new Date(2024, 4, 5),
    showEnd: false,
    showTime: false,
  },
  {
    content: "線上投票",
    start: new Date(2024, 4, 6, 18, 0, 0),
    end: new Date(2024, 4, 8, 23, 59, 59),
    showEnd: true,
    showTime: true,
  },
  {
    content: "當選名單公告",
    start: new Date(2024, 4, 9),
    end: new Date(2024, 4, 10),
    showEnd: false,
    showTime: false,
  },
];
</script>
