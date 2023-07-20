<script lang="ts">
    import ComfyJS from "comfy.js";
	import { onMount } from "svelte";

    interface ChatMessage {
        user: string;
        message: string;
    }

    onMount(() => {
        ComfyJS.Init("dydevx");
    });

    let chatMessages: ChatMessage[] = [];
    $: ComfyJS.onChat = (user, message, _flags, _self, _extra) => {
        chatMessages.push({ user, message });
    }
</script>

<div id="chat">
    <ul>
        {#each chatMessages as {user, message}}
            <li>
                <h5>{user}</h5>
                <blockquote>{message}</blockquote>
            </li>
        {/each}
    </ul>
</div>

<style>
    #chat {
        width: 400px;
        height: 350px;
        overflow-y: auto;
        display: flex;
        flex-direction: column-reverse;
    }

    #chat::-webkit-scrollbar {
        display: none;
    }

    #chat ul {
        list-style-type: none;
        list-style-position: outside;
    }

    #chat li {
        background-color: hsl(196, 58%, 93%);
        box-sizing: border-box;
        padding: 1rem 10px;
        margin-bottom: 10px;
    }

    @keyframes slide-in-left {
        from {
            transform: translateX(400px);
            opacity: 0;
        }

        to {
            transform: translateX(0);
            opacity: 1;
        }
    }

    #chat li:last-of-type {
        animation-name: slide-in-left;
        animation-duration: 0.15s;
        animation-timing-function: ease-in;
    }

    #chat li:not(:last-of-type) {
        opacity: 0.8;
    }

</style>