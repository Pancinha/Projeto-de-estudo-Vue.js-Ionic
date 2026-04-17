<script setup>
import {  
  IonPage,
  IonContent,
  IonCard,
  IonCardContent,
  IonInput,
  IonButton,
  IonText } from '@ionic/vue';

import { ref } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();

const email = ref('');
const senha = ref('');
const erro = ref('');

function login() {
   if (email.value === 'admin' && senha.value === 'admin') {
    console.log(`Email: ${email.value}, Senha: ${senha.value}`);
    router.push('/folder/inbox')  } else {
    erro.value = 'Email ou senha incorretos';
    return erro.value;
  }
}

</script>

<template>
  <IonPage class="ion-padding">
    <IonContent class="login-container">
      <div class="login-box">
        <h2>Login</h2>
        <IonCard>
          <IonCardContent>
            <IonInput
              label="Email"
              placeholder="Digite seu email"
              v-model="email"
            ></IonInput>
            <IonInput
              label="Senha"
              placeholder="Digite sua senha"
              type="password"
              v-model="senha"
            ></IonInput>
            <IonButton expand="block" @click="login">Entrar</IonButton>
            <IonText color="danger" v-if="erro">{{ erro }}</IonText>
          </IonCardContent> 
        </IonCard>
      </div> 
    </IonContent>
  </IonPage>
</template>

<style scoped>
.login-container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  background: linear-gradient(180deg, #f4f5f8 0%, #e9edf5 100%);
}

ion-card {
  width: 100%;
  max-width: 400px;
  border-radius: 20px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.12);
  padding: 12px;
}

ion-card-content {
  display: flex;
  flex-direction: column;
  gap: 18px;
}

ion-input {
  --background: #ffffff;
  --border-radius: 12px;
  --padding-start: 12px;
  --padding-end: 12px;
  border: 1px solid #dcdfe5;
  border-radius: 12px;
}

ion-button {
  margin-top: 10px;
  --border-radius: 12px;
  height: 48px;
  font-size: 16px;
  font-weight: 600;
}

ion-text {
  display: block;
  text-align: center;
  margin-top: 8px;
  font-size: 14px;
}

ion-content.login-container {
  --background: #0d0d0d;
}

.login-container::part(scroll) {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
}

ion-card {
  width: 100%;
  max-width: 420px;
  border-radius: 24px;
  padding: 16px;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.25);
}
.login-box h2 {
  text-align: center;
  color: #333;
  margin-bottom: 20px;
  font-size: 32px;
  font-weight: bold;
}
</style>