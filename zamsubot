const { Client, GatewayIntentBits } = require('discord.js');
const { joinVoiceChannel } = require('@discordjs/voice');
const express = require('express');

// 1. 무료 서버가 잠들지 않게 도와주는 미니 웹 서버
const app = express();
app.get('/', (req, res) => res.send('디스코드 봇이 24시간 정상 작동 중입니다!'));
app.listen(process.env.PORT || 3000, () => {
    console.log('웹 서버 실행 완료');
});

// 2. 디스코드 봇 세팅 (채팅을 안 읽으므로 최소한의 권한만 사용)
const client = new Client({
    intents: [GatewayIntentBits.Guilds, GatewayIntentBits.GuildVoiceStates]
});

// 3. 봇이 켜지자마자 알아서 통화방에 입장하는 로직
client.once('ready', () => {
    console.log(`✅ [${client.user.tag}] 봇 준비 완료!`);

    const guildId = process.env.GUILD_ID;
    const channelId = process.env.CHANNEL_ID;

    const guild = client.guilds.cache.get(guildId);
    if (guild) {
        joinVoiceChannel({
            channelId: channelId,
            guildId: guildId,
            adapterCreator: guild.voiceAdapterCreator,
            selfDeaf: true, // 봇이 다른 사람의 소리를 듣지 않게 설정 (서버 자원 절약)
            selfMute: true  // 봇의 마이크를 끔
        });
        console.log('🎙️ 통화방 자동 입장 완료! (절대 나가지 않음)');
    } else {
        console.error('❌ 봇이 서버를 찾을 수 없습니다. 서버 ID를 올바르게 넣었는지 확인하세요.');
    }
});

client.login(process.env.DISCORD_TOKEN);
