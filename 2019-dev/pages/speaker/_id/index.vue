<template>
  <div class="speaker">
    <div class="speaker__banner"></div>
    <div class="section">
      <h2 class="title"><span>{ 驅動高速資訊交流圈</span><span class="color-primary">的佼佼者們</span><span>; }</span></h2>
      <p class="intro">每年我們都邀請了超過 25 位講師來到現場，包含知名的前端開發者、軟體工程師、資深架構師、設計師，以及專案經理等等，讓這場活動更加多元，也貫徹 MOPCON 的精神，使參與的會眾都能從中獲得養分，甚至達到近一步的交流。</p>
      <SectionMasterSpeaker id="sectionMasterSpeaker" :speakerData="speakerData" :speakerId="speakerId" />
    </div>
    <SectionApp />
  </div>
</template>

<script>
import SectionMasterSpeaker from './SectionMasterSpeaker';
import SectionApp from '~/components/SectionApp';

export default {
  name: 'speakerDetail',
  head() {
    let title = `sdfsd講者 | MOPCON 2019`;
    let description = `MOPCON 2019 堅持濁水溪以南，南台灣最大行動科技年會，10/19-10/20 高雄國際會議中心與您見面。持續匯集知識與人才，打造高速資訊交流圈，提升個人與團隊價值，讓知識影響正向循環。`;
    let url = `${process.env.BASE_URL}/2019/speaker`;
    let image = `https://mopcon.org/2019/banner.png`;

    if (this.speakerData) {
      title = `${this.speakerData.name} |sdfsd 講者 MOPCON 2019`;
      description = `${this.speakerData.summary}`;
      url = `${process.env.BASE_URL}/2019/speaker/${this.speakerData.speaker_id}`;
      image = `${this.speakerData.img.web}`;
    }
    return {
      title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: description,
        },
        // fb
        {
          hid: 'og-title',
          property: 'og:title',
          content: title,
        },
        {
          hid: 'og-description',
          property: 'og:description',
          content: description,
        },
        {
          hid: 'og-url',
          property: 'og:url',
          content: url,
        },
        {
          hid: 'og-image',
          property: 'og:image',
          content: image,
        },
        // twitter seo
        {
          hid: 'twitter-site',
          name: 'twitter:site',
          content: title,
        },
        {
          hid: 'twitter-description',
          name: 'twitter:description',
          content: description,
        },
        {
          hid: 'twitter-app:name:iphone',
          name: 'twitter:app:name:iphone',
          content: title,
        },
        {
          hid: 'twitter-app:name:ipad',
          name: 'twitter:app:name:ipad',
          content: title,
        },
      ],
    };
  },
  components: {
    SectionMasterSpeaker,
    SectionApp,
  },
  data() {
    return {
      speakerData: null,
      speakerId: '',
    };
  },
  async asyncData({ $axios, params, redirect }) {
    try {
      const { success, data } = await $axios.$get(
        `${process.env.BASE_URL}/api/2019/speaker/${params.id}`
      );
      // 如果失敗就返回 /speaker
      if (success) {
        return { speakerData: data, speakerId: params.id };
      } else {
        redirect(`/speaker`);
      }
    } catch (err) {
      redirect(`/speaker`);
    }
  },
};
</script>

<style lang="scss" src="./style.scss" scoped></style>
