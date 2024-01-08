<template>
    <label class="custom-file-upload">
        <input type="file" @change="handleFileChange"/>
        <img :src="'storage/' + pfp " class="my-pfp rounded-full h-20 w-20" />

        <svg class="edit-icon" width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
        <g id="Edit / Edit_Pencil_Line_01">
        <path id="Vector" d="M4 20.0001H20M4 20.0001V16.0001L12 8.00012M4 20.0001L8 20.0001L16 12.0001M12 8.00012L14.8686 5.13146L14.8704 5.12976C15.2652 4.73488 15.463 4.53709 15.691 4.46301C15.8919 4.39775 16.1082 4.39775 16.3091 4.46301C16.5369 4.53704 16.7345 4.7346 17.1288 5.12892L18.8686 6.86872C19.2646 7.26474 19.4627 7.46284 19.5369 7.69117C19.6022 7.89201 19.6021 8.10835 19.5369 8.3092C19.4628 8.53736 19.265 8.73516 18.8695 9.13061L18.8686 9.13146L16 12.0001M12 8.00012L16 12.0001" stroke="#000000" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        </g>
        </svg>
    </label>
</template>

<script setup>
defineProps({
    pfp: {
        type: String,
    },
    handleFileChange: {
        type: Function,
    },
});
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
