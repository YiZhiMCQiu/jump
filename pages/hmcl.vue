<script setup>
import HmclDownloadCard from "~/components/HmclDownloadCard.vue";

async function fetchData(url) {
  try {
    const response = await fetch(url);
    const data = await response.json();
    return JSON.parse(atob(data.content));
  } catch (error) {
    console.error('Error:', error.message);
    throw error;
  }
}

const urls = {
  stable: 'https://api.github.com/repos/HMCL-dev/HMCL-Update/contents/update/stable.json',
  dev: 'https://api.github.com/repos/HMCL-dev/HMCL-Update/contents/update/dev.json',
  prs: 'https://api.github.com/repos/burningtnt/HMCL-Snapshot-Update/contents/artifacts/v5/uploaders/8mi.139/HMCL-dev/HMCL/main/gradle.yml.jar.json'
};

const [stable, dev, prs] = await Promise.all([
  fetchData(urls.stable),
  fetchData(urls.dev),
  fetchData(urls.prs)
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