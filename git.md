# GIT #

## Repositorio ##
* Siempre existirán dos branches en el repositorio: `master` y `develop`
* `master` mantendrá la versión lista para producción. Cualquier cambio que entre a `master` está listo para ser (y deberá ser) desplegado en producción
* `develop` mantendrá la versión estable de desarrollo. Los *features* serán *merged* a develop, allí se verificarán los cambios en *staging* y posteriormente pasarán a `master` para ser desplegados en producción. 

## Comenzar un nuevo *feature* ##
Crear un nuevo feature branch desde la punta de `develop`.

```
git checkout develop
git pull
git checkout -b feature/<nombre>
```

Trabajar en el *feature*, agregar cambios y hacer *commits*.

```
git add --all
git commit
```

Publicar el *branch* para compartir con el equipo. Realizar un *rebase* interactivo a la punta de `develop` antes de publicar. Juntar (*squash*) *commits* innecesarios o reduntantes, o modificar los comentarios.

```
git fetch origin
git rebase -i origin/develop

git push origin feature/<nombre>
```

*Una vez publicado el feature branch en el origen, evitar hacer rebase, excepto justo antes de hacer merge.*

## Revisión de código ##

Crear un *pull request* a `develop` para revisión.

El *reviewer* revisa los cambios localmente y se discuten en *Bitbucket*.

```
git checkout feature/<nombre>
git diff staging/master..HEAD
```

## Merge a develop ##

Cuando los cambios están aprobados, hacer un último *rebase* sobre `develop`.

```
git fetch origin
git rebase -i origin/develop
```

Ver lista de cambios.

```
git log origin/develop..feature/<nombre>
git diff --stat origin/develop
```

Merge a `develop`.

```
git checkout develop
git merge feature/<nombre> --no-ff
git push
```

Eliminar *feature branch*.

```
git push origin --delete feature/<nombre>
git branch -d feature/<nombre>
```

En este punto se realizan pruebas en *staging*.

## Producción ##

Una vez verificados los cambios en *staging*, aplicarlos a `master`.

```
git checkout master
git merge develop --ff-only
git push
```

Hacer *deploy* a producción.


#### Referencias ####
* [Git flow](http://nvie.com/posts/a-successful-git-branching-model/)
* [Github flow](http://nvie.com/posts/a-successful-git-branching-model/)
* [Comparing workflows](https://www.atlassian.com/git/tutorials/comparing-workflows/)
* [Interactive rebase](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase-i)