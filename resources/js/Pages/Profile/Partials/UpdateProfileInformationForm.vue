<script setup>
import InputError from '@/Components/InputError.vue';
import InputLabel from '@/Components/InputLabel.vue';
import PrimaryButton from '@/Components/PrimaryButton.vue';
import TextInput from '@/Components/TextInput.vue';
// import PfpInput from '@/Components/PfpInput.vue';
import { Link, useForm, usePage } from '@inertiajs/vue3';
import {reactive, toRefs} from 'vue';

defineProps({
    mustVerifyEmail: {
        type: Boolean,
    },
    status: {
        type: String,
    },
});

const user = usePage().props.auth.user;

const form = useForm({
    name: user.name,
    email: user.email,
    pfp: user.pfp,
});

const state = reactive({
  newPfp: null,
});

const handleFileChange = (event) => {
    URL.revokeObjectURL(state.newPfp);
    state.newPfp = null;
    form.pfp = event.target.files[0];
    console.log(form.pfp);
    state.newPfp = URL.createObjectURL(event.target.files[0]);
};

</script>

<style scoped>
.custom-file-upload {
    display: inline-block;
    cursor: pointer;
}

input[type="file"] {
    display: none;
}

.my-pfp {
    width: 100px;
    height: 100px;
    border-radius: 50%;
    object-fit: cover;
    object-position: center;
}

.my-pfp:hover {
    opacity: 0.5;
}

.edit-icon {
  position: relative;
  top: -50px;
  left: 50%;
  transform: translate(-50%, -50%);
  opacity: 0; /* Initially hidden */
  cursor: pointer;
}

.custom-file-upload:hover .edit-icon {
  opacity: 1;
}
</style>

<template>
    <section>
        <header>
            <h2 class="text-lg font-medium text-gray-900">Profile Information</h2>

            <p class="mt-1 text-sm text-gray-600">
                Update your account's profile information and email address.
            </p>
        </header>

        <form @submit.prevent="form.post(route('profile.update'))" class="mt-6 space-y-6">

            <!-- <PfpInput
            id="pfp"
            :pfp="form.pfp"
            :handleFileChange="handleFileChange" /> -->

            <label class="custom-file-upload">
                <input id="pfp" type="file" @change="handleFileChange"/>
                <img :src="state.newPfp ? state.newPfp : 'storage/' + form.pfp" class="my-pfp rounded-full h-20 w-20" />

                <svg class="edit-icon" width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                <g id="Edit / Edit_Pencil_Line_01">
                <path id="Vector" d="M4 20.0001H20M4 20.0001V16.0001L12 8.00012M4 20.0001L8 20.0001L16 12.0001M12 8.00012L14.8686 5.13146L14.8704 5.12976C15.2652 4.73488 15.463 4.53709 15.691 4.46301C15.8919 4.39775 16.1082 4.39775 16.3091 4.46301C16.5369 4.53704 16.7345 4.7346 17.1288 5.12892L18.8686 6.86872C19.2646 7.26474 19.4627 7.46284 19.5369 7.69117C19.6022 7.89201 19.6021 8.10835 19.5369 8.3092C19.4628 8.53736 19.265 8.73516 18.8695 9.13061L18.8686 9.13146L16 12.0001M12 8.00012L16 12.0001" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
                </g>
                </svg>
            </label>

            <div>
                <InputLabel for="name" value="Name" />

                <TextInput
                    id="name"
                    type="text"
                    class="mt-1 block w-full"
                    v-model="form.name"
                    required
                    autofocus
                    autocomplete="name"
                />

                <InputError class="mt-2" :message="form.errors.name" />
            </div>

            <div>
                <InputLabel for="email" value="Email" />

                <TextInput
                    id="email"
                    type="email"
                    class="mt-1 block w-full"
                    v-model="form.email"
                    required
                    autocomplete="username"
                />

                <InputError class="mt-2" :message="form.errors.email" />
            </div>

            <div v-if="mustVerifyEmail && user.email_verified_at === null">
                <p class="text-sm mt-2 text-gray-800">
                    Your email address is unverified.
                    <Link
                        :href="route('verification.send')"
                        method="post"
                        as="button"
                        class="underline text-sm text-gray-600 hover:text-gray-900 rounded-md focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
                    >
                        Click here to re-send the verification email.
                    </Link>
                </p>

                <div
                    v-show="status === 'verification-link-sent'"
                    class="mt-2 font-medium text-sm text-green-600"
                >
                    A new verification link has been sent to your email address.
                </div>
            </div>

            <div class="flex items-center gap-4">
                <PrimaryButton :disabled="form.processing">Save</PrimaryButton>

                <Transition
                    enter-active-class="transition ease-in-out"
                    enter-from-class="opacity-0"
                    leave-active-class="transition ease-in-out"
                    leave-to-class="opacity-0"
                >
                    <p v-if="form.recentlySuccessful" class="text-sm text-gray-600">Saved.</p>
                </Transition>
            </div>
        </form>
    </section>
</template>
