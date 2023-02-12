

<template>
    <div class="no-data">
        <img src="../assets/svgs/Empty-pana.svg" alt="">
        <h3>OMG!</h3>
        <p>You have no Note, let's change that!</p>
    </div>
</template>


<style scoped>
.no-data {
    max-width: 500px;
    width: 100%;

    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;

    margin: auto;
    text-align: center;
}
img {
    width: 100%;
}

h3 {
    font-size: 3rem;
}
p {
    font-size: 1.5rem;
    line-height: 1.1;
}

@media only screen and (max-width: 601px) {
    h3 {
    font-size: 2.5rem;
}
p {
    font-size: 1rem;
    line-height: 1.1;
}
}


</style>