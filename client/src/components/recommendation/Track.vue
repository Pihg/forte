<template>
    <div class="card h-100 w-100 border-0" @contextmenu.prevent="right_click({ item: props.track, event: $event })">
        <div class="p-3">
            <div class="position-relative clickable-shadow rounded">
                <div @click="openTrack">
                    <img class="img-fluid placeholder-img rounded" :src="get_cover(props.track.cover)"
                        @error="placeholder" />
                </div>
                <div class="position-absolute bottom-0 right-0 m-2">
                    <button class="btn btn-light action-btn bi bi-play-fill" type="button" @click="play(props.track.id)">
                    </button>
                </div>
            </div>
            <div class="d-flex flex-fill">
                <h6 class="theme-color purple-on-hover fw-bold text-break text-wrap clickable p-2 ps-0" @click="openTrack">
                    {{
                        props.track.title
                    }}</h6>
            </div>
        </div>
    </div>
</template>

<script setup>
import { useRouter } from 'vue-router';
import { store } from '/js/store.js';
import { right_click, action } from '/js/events.js';

const router = useRouter();

async function placeholder(obj) {
    obj.target.src = "/images/track.svg";
}

function get_cover(cover) {
    if (!cover) {
        return "/images/track.svg"
    }

    if (cover.startsWith("http")) {
        return cover;
    }

    return ft.server + '/' + cover;
}

async function openTrack() {
    store.selected_track_id = props.track.id;
    router.push("/album/" + props.track.album);
}

// Must be synchronized in groupSession: ok
async function play(id) {
    action({
        func: async function op() {
            ft.playTrack(id);
        },
        object: [id],
        operation: "playTrack",
    })
}

const props = defineProps({
    track: {
        type: Object,
    }
});
</script>