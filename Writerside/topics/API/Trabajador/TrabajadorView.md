# Trabajador - Views

## GET /trabajadores
> Returns a view with all the workers in the database
> [Trabajadores/indexTrabajadores](http://localhost:8080/trabajadores)

![trabajadores-table.png](trabajadores-table.png)

***

## GET /trabajadores/:id
> Returns a view with the detailed information of a specific worker  
> [Trabajadores/trabajadoresDetalle](http://localhost:8080/trabajadores/T001)

![trabajadores-detail.png](trabajadores-detail.png)

***

## GET /trabajadores/nuevo
> Returns a view with a form to create a new worker
> [Trabajadores/trabajadoresForm](http://localhost:8080/trabajadores/nuevo)

![trabajadores-add.png](trabajadores-add.png)

***

## GET /trabajadores/editar
> Returns a view with the same form that 'trabajadores/nuevo', but modified to edit a worker
> [Trabajadores/trabajadoresForm](http://localhost:8080/trabajadores/editar?idTrabajador=T001)

![trabajadores-edit.png](trabajadores-edit.png)

***

## GET /trabajadores/processForm
> This endpoint is not meant to be accessed directly,
> but to process the form in 'trabajadores/nuevo' and 'trabajadores/editar'  
> It checks if the method is POST or PUT and redirects to the corresponding endpoint

***

## POST /trabajadores/create
> Creates a new worker in the database  
> If there is no error, redirects to the 'ready' view with a success message  
> If there is an error, redirects to the 'trabajadores/nuevo' view with an error message

<tabs>
    <tab title="Successfully worker added">
        <img width="600" border-effect="rounded" alt="Add success screenshot" src="trabajadores-add-success.png"/>
    </tab>
    <tab title="Error adding a worker">
        <img width="600" border-effect="rounded" alt="Add error screenshot" src="trabajadores-add-error.png"/>
    </tab>
</tabs>


***

## PUT /trabajadores/edit
> Updates the information of a worker in the database
> If there is no error, redirects to the 'ready' view with a success message
> If there is an error, redirects to the 'trabajadores/editar' view with an error message

<tabs>
    <tab title="Successfully worker edited">
        <img width="600" border-effect="rounded" alt="Edit success screenshot" src="trabajadores-edit-success.png"/>
    </tab>
    <tab title="Error editing a worker">
        <img width="600" border-effect="rounded" alt="Edit error screenshot" src="trabajadores-edit-error.png"/>
    </tab>
</tabs>


***

## DELETE /trabajadores/delete
> Deletes a worker from the database  
> Redirects to the 'ready' view with a success message  
> Workers with tasks assigned cannot be deleted

<tabs>
    <tab title="Successfully worker deleted">
        <img width="600" border-effect="rounded" alt="Delete success screenshot" src="trabajadores-delete-success.png"/>
    </tab>
    <tab title="Error deleting a worker">
        <img width="600" border-effect="rounded" alt="Delete error screenshot" src="trabajadores-delete-error.png"/>
    </tab>
</tabs>

