<script>
    import NavbarC from './components/NavbarC.svelte';
    import { v4 as uuidv4 } from 'uuid';
    import Swal from 'sweetalert2';
    import toastr from 'toastr';

    let product = {
        id: '',
        name: '',
        description: '',
        category: '',
        imgURL: '',
    };
    let activeEdit = false;
    let listProducts = [
        {
            id: 1,
            name: 'Laptop Hp',
            description: 'Una laptop fenomenal',
            category: 'laptops',
            imgURL: '',
        },
        {
            id: 2,
            name: 'PC Gaming',
            description: 'Pc Economica',
            category: 'computer',
            imgURL: '',
        },
    ];
    const onSubmitHandler = () => {
        if (!activeEdit) {
            addProduct();
        } else {
            updateProduct(product);
        }
    };
    const addProduct = () => {
        listProducts = listProducts.concat({
            ...product,
            id: uuidv4(),
        });
    };
    const deleteProduct = id => {
        showAlert(
            () => {
                listProducts = listProducts.filter(e => e.id !== id);
                showNotification('Producto Eliminado con Exito');
            },
            'Esta seguro de borrarlo?',
            'Si borralo',
        );
    };
    const updateProduct = product => {
        showAlert(
            () => {
                let productIndex = listProducts.findIndex(
                    e => e.id == product.id,
                );
                listProducts[productIndex] = product;
                activeEdit = false;
                showNotification(
                    `Se Actualizo el Producto ${updatedProduct.name}`,
                );
            },
            'Esta seguro de Actualizarlo?',
            'Si Actualizalo',
        );
    };
    const editActive = productUpdated => {
        activeEdit = true;
        product = productUpdated;
    };
    const showAlert = (callback, title, confirmButtonText) => {
        Swal.fire({
            title,
            text: 'Este cambio no es reversible!',
            icon: 'warning',
            showCancelButton: true,
            confirmButtonColor: '#3085d6',
            cancelButtonColor: '#d33',
            confirmButtonText,
        }).then(result => {
            callback();
        });
    };
    const showNotification = text => {
        toastr.success(text);
    };
</script>

<main>
      <NavbarC />
    <div class="container my-4">
        <div class="row">
            <div class="col-md-4">
                <form on:submit|preventDefault={onSubmitHandler}>
                    <div class="form-group">
                        <label for="exampleInputEmail1">Name</label>
                        <input
                            type="text"
                            required
                            class="form-control"
                            placeholder="Enter Name"
                            bind:value={product.name} />
                    </div>
                    <div class="form-group">
                        <label for="exampleInputPassword1">Description</label>
                        <input
                            type="text"
                            required
                            class="form-control"
                            placeholder="Enter Description"
                            bind:value={product.description} />
                    </div>
                    <div class="form-group">
                        <label for="exampleInputPassword1">Seleccione una
                            categoria</label>
                        <select
                            class="form-control"
                            required
                            id="exampleFormControlSelect1"
                            bind:value={product.category}>
                            <option value="laptops" selected>Laptops</option>
                            <option value="peripherials">Peripherials</option>
                            <option value="servers">Servers</option>
                            <option value="computer">Computer</option>
                        </select>
                    </div>
                    <div class="form-group">
                        <label for="exampleInputPassword1">ImgUrl</label>
                        <input
                            type="text"
                            required
                            class="form-control"
                            placeholder="Enter ImageURL"
                            bind:value={product.imgURL} />
                    </div>
                    {#if !activeEdit}
                        <button
                            type="submit"
                            class="btn btn-success btn-block">Save Product</button>
                    {:else}
                        <button
                            type="submit"
                            class="btn btn-success btn-block">Edit Product</button>
                    {/if}
                </form>
            </div>
            <div class="col-md-8">
                {#each listProducts as p}
                    <div class="p-2 my-2 card">
                        <div class="row">
                            <div class="my-auto text-center col-md-4">
                                {#if !p.imgURL}
                                    <img
                                        src="/favicon.png"
                                        alt=""
                                        class="img-fluid" />
                                {:else}
                                    <img
                                        src={p.imgURL}
                                        alt=""
                                        class="img-fluid" />
                                {/if}
                            </div>
                            <div class="col-md-8 card-body">
                                <h5>
                                    <strong
                                        class="text-capitalize">{p.name}</strong>
                                    <span>
                                        <small
                                            class="text-small text-capitalize">({p.category})</small>
                                    </span>
                                </h5>
                                <p class="card-text text-capitalize">
                                    {p.description}
                                </p>
                                <button
                                    type="button"
                                    class="btn btn-danger btn-sm"
                                    on:click={deleteProduct(p.id)}>Delete</button>
                                <button
                                    type="button"
                                    class="btn btn-info btn-sm"
                                    on:click={editActive(p)}>Edit</button>
                            </div>
                        </div>
                    </div>
                {/each}
            </div>
        </div>
    </div>
</main>
