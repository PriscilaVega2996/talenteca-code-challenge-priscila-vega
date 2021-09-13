<script>
    //Variable que almacena los datos de la api
    let plans = getPlans();
    //Bandera para saber que la moneda es USD
    let isUSD = true;
    //Bandera para saber que la moneda es MXN
    let isMXN = false;
    //Indice para recorrer el arreglo de precios
    let index = 0;


    //Funcion para cambiar a USD
    const changeUSD = e => {
        e.preventDefault()
        isUSD = true;
        isMXN = false;
        index = 0;
    }

    //Funcion para cambiar a MXN
    const changeMXN = e => {
        e.preventDefault()
        isUSD = false
        isMXN = true
        index = 1;
    }

    //Funcion con fetch para consumir la API
    async function getPlans() {
        const response = await fetch("https://www.talenteca.com/api/v1/membership-plans");
        const plansObj = await response.json();

        if (response.ok) {
            return plansObj;
        } else {
            throw new Error(plansObj);
        }
    }
</script>

<main>
    <!--Navbar de boostrap-->
    <!--Navbar-->
    <nav class="navbar navbar-expand-sm bg-secondary navbar-dark">
        <!-- Brand/logo -->
        <a class="navbar-brand" href="/">
            <img src="https://cdn1.talenteca.com/assets/salmon-20210903150520_1479_6c4be8d3a20e3f1e4e9e0b4ce184fe716663d9b8_1630681520000_24916/images/logo.png"
                 alt="logo" style="width:160px;">
        </a>

        <!-- Links -->
        <ul class="navbar-nav ml-auto">
            <li class="nav-item">
                <a style="color: #000000" class="nav-link" href="/" on:click={changeUSD}>USD</a>
            </li>
            <li class="nav-item">
                <a style="color: #000000" class="nav-link" href="/" on:click={changeMXN}>MEX</a>
            </li>
        </ul>
    </nav>
    <!--Hero - Jumbotron-->
    <section>
        <div class="jumbotron jumbotron-fluid">
            <div class="container">
                <h1 class="display-4" style="font-weight: bold">TALENTECA CODE CHALLENGE</h1>
                <p class="lead">
                    This is a challenge proposed to be part of the <strong>Talenteca</strong> team, I used Bootstrap 4 as CSS Framework. <br><br> <strong>Made by:</strong> Priscila Vega</p>
            </div>
        </div>
    </section>
    <!--Contenido General-->
    <section>
        <div class="container">
            <div class="row">
                {#await plans}
                    <p>...waiting</p>
                {:then membership_plans}
                    {#each membership_plans as plan}
                        <div class="col-sm p-4">
                            <div class="card" style="width: 18rem;">
                                <div class="card-header">
                                    {plan.name}
                                </div>
                                <ul class="list-group list-group-flush">
                                    <li class="list-group-item"><strong>Number of units:</strong>
                                        <span>{plan.number_of_units}</span></li>
                                    <li class="list-group-item"><strong>Billing
                                        Frequency:</strong> {plan.billing_frequency}</li>
                                    <li class="list-group-item"><strong>Currency:</strong> {plan.prices[index].currency}
                                    </li>
                                    <li class="list-group-item"><strong>Price per unit:</strong>
                                        <span>{plan.prices[index].currency_symbol}</span>{plan.prices[index].per_unit}
                                    </li>
                                    <li class="list-group-item"><strong>Total Price:</strong>
                                        <span>{plan.prices[index].currency_symbol}</span>{plan.prices[index].total}</li>
                                </ul>
                            </div>
                        </div>
                    {/each}
                {:catch error}
                    <p style="color: red">{error.message}</p>
                {/await}
            </div>
        </div>
    </section>
</main>

<style>

</style>