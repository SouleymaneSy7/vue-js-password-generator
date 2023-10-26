<template>
  <Teleport to="body">
    <div
      class="notification"
      :class="showNotification === true ? 'active' : ''"
    >
      <p class="notification__msg">{{ notificationMsg }}</p>
    </div>

    <div
      class="notification notification--errors"
      :class="errorsNotifications === true ? 'active' : ''"
    >
      <p class="notification__msg">{{ notificationErrorsMsg }}</p>
    </div>
  </Teleport>
</template>

<script setup>
defineProps({
  showNotification: {
    type: Boolean,
    default: false,
  },
  errorsNotifications: {
    type: Boolean,
    default: false,
  },
  notificationErrorsMsg: {
    type: String,
  },
  notificationMsg: {
    type: String,
  },
});
</script>
