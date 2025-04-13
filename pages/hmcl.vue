<script setup>
import HmclDownloadCard from "~/components/HmclDownloadCard.vue";

async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`Failed to fetch: ${response.status} ${response.statusText}`);
    }
    const data = await response.json();
    if (typeof data.content === 'string' && data.encoding === 'base64') {
      return JSON.parse(atob(data.content));
    }
    return data;
  } catch (error) {
    console.error('Error:', error.message);
    throw error;
  }
}

const urls = {
  stable: 'https://api.codetabs.com/v1/proxy/?quest=https://gitee.com/Glavo/HMCL-Update/raw/main/update/stable.json',
  dev: 'https://api.codetabs.com/v1/proxy/?quest=https://gitee.com/Glavo/HMCL-Update/raw/main/update/dev.json'
};

const [stable, dev, prs] = await Promise.all([
  fetchData(urls.stable),
  fetchData(urls.dev),
  fetchData("https://hmcl-snapshot-update-73w.pages.dev/v5/uploaders/8mi.139/burningtnt/HMCL/prs/gradle.yml.jar.json")
]);

const versions = [
  {title: '稳定版', url: stable.jar, version: stable.version},
  {title: '开发版', url: dev.jar, version: dev.version},
  {title: 'PR Collection', url: prs.jar, version: prs.version}
];
</script>

<template>
  <title>HMCL</title>
  <NuxtLayout>
    <v-app>
      <v-container class="fill-height">
        <v-row justify="center" align="center">
          <v-col
              v-for="(version, index) in versions"
              :key="index"
              cols="12"
              sm="8"
              md="6"
              lg="4"
              class="mb-4"
          >
            <HmclDownloadCard
                :name="version.title"
                :url="version.url"
                :version="version.version"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-app>
  </NuxtLayout>
</template>