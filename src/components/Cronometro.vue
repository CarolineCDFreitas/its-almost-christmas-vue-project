<script setup>
import { computed, reactive, ref } from "vue";
import NumberFlow, { NumberFlowGroup } from '@number-flow/vue';

let currentTime = ref(new Date());
const christmasTime = new Date("2024-12-25T00:00:00");
let difference = computed(() => christmasTime - currentTime.value);

const second = 1000;
const minute = second * 60;
const hour = minute * 60;
const day = hour * 24;

const remaining = reactive({
    Days: 0,
    Hours: 0,
    Minutes: 0,
    Seconds: 0,
});

let message = ref("");

const countdown = () => {
    if (difference.value > 0) {
        (remaining.Days = Math.floor(difference.value / day));
        (remaining.Hours = Math.floor((difference.value % day) / hour));
        (remaining.Minutes = Math.floor((difference.value % hour) / minute));
        (remaining.Seconds = Math.floor((difference.value % minute) / second));
    } else {
        message.value = "Feliz Natal";
    }
};


let updating = setInterval(() => {
    currentTime.value = new Date();
    countdown();

    if (difference.value <= 0) {
        clearInterval(updating);
    }
}, 1000);
</script>

<template>
    <section>
        <div>
            <NumberFlowGroup>
                <NumberFlow :trend="-1" :value="remaining.Days" :format="{ minimumIntegerDigits: 1 }" />
                <span>d - </span>
                <NumberFlow :trend="-1" :value="remaining.Hours" :format="{ minimumIntegerDigits: 1 }" />
                <span>h - </span>
                <NumberFlow :trend="-1" :value="remaining.Minutes" :format="{ minimunIntegerDigits: 1 }" />
                <span>m - </span>
                <NumberFlow :trend="-1" :value="remaining.Seconds" :format="{ minimumIntegerDigits: 1 }" />
                <span>s</span>
            </NumberFlowGroup>
            <p>{{ message }}</p>
        </div>


    </section>
</template>

<style scoped lang="scss">
number-flow-vue::part(number),
span {
    color: #CD3C32;
    font-weight: 600;
    font-size: clamp(2rem, 5vw, 4rem);
    line-height: clamp(1.2, 1.4, 1.5);
}
</style>
