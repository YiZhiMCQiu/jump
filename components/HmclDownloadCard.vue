<script setup>
const props = defineProps({
  name: String,
  url: String,
  version: String,
});

function download(type) {
  if (!props.url) {
    console.error('Download URL is missing.');
    return;
  }

  const link = document.createElement('a');
  link.href = props.url.replace("maven.aliyun.com/repository/central/", "mirrors.cloud.tencent.com/nexus/repository/maven-public/");
  if (type === "exe") link.href = link.href.replace(".jar", ".exe")
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}

function open(url){
  const link = document.createElement('a');
  link.target = '_blank';
  link.href = url;
  document.body.appendChild(link);
  link.click();
  document.body.removeChild(link);
}
</script>

<template>
  <v-card
      color="#5C6BC0"
      variant="elevated"
      class="mx-auto"
      hover
  >
    <v-card-item>
      <div>
        <div class="text-overline mb-1">
          {{ version }}
        </div>
        <div class="text-h6 mb-1">
          {{ name }}
        </div>
      </div>
    </v-card-item>

    <v-card-actions>
      <v-btn @click="download('exe')" variant="tonal">
        下载[EXE]
      </v-btn>
      <v-btn @click="download('jar')" variant="outlined">
        下载[JAR]
      </v-btn>
      <v-btn variant="outlined" v-if="name.includes('Collection')" @click="open('https://github.com/burningtnt/HMCL/pull/9')">
        详情
      </v-btn>
    </v-card-actions>
  </v-card>
</template>