# When Python meets GraphQL: Managing contributors identities in your open source project

https://fosdem.org/2020/schedule/event/python2020_graphql/

https://fosdem.org/2020/schedule/event/python2020_graphql/attachments/slides/3770/export/events/attachments/python2020_graphql/slides/3770/slides_when_python_meets_graphql_sortinghat.pdf

Discussed a product that his team built -- SortingHat.

It's a Python Django app that conforms to the GraphQL API.

It uses `graphene-django` plugin with provides some additional abstracts on top of Django for implementing GraphQL backends.

Implement Django models, schemas and API/resolvers in terms of `graphene-django` abstractions.

Can provide the GraphQL backend authentication using Javascript Web Token (JWT)s, using the `graphene-jwt` package.

Great support for pagination too.
