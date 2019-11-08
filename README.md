# gestion_admin

```python
  from django.contrib import admin
  #-------Importation----------#
  from . import models
  from django.utils.safestring import mark_safe

  #-----------Register inline--------------#
  #-----------Permet lister d'autre(s) ModelAdmin associé à celui-ci--------------#

  class SousCategorieInline(admin.TabularInline):
      model =  models.SousCategorie
      #-------------le nombre d element a ajouter----------------#
      extra = 0

  class produitInline(admin.TabularInline):
      model = models.Produit
      extra = 0





  #------------register simple------------#

  @admin.register(models.Categorie)
  class CategorieAdmin(admin.ModelAdmin):
  #-----------Permet lister SousCategorieInline ModelAdmin associé à CategorieAdmin--------------#

      inlines = [SousCategorieInline]
  #-----------Permet d afficher les champs dans l admin--------------#

      list_display = (
          'view_image',
          'nom',
          'status',
          'date_add',
          'date_upd',
      )
  #-----------Permet de filtrer les champs dans l admin--------------#
      list_filter = (
          'status',
          'date_add',
          'date_upd',
      )

      #-------- fonction pour afficher une image dans l admin-----------------#
      def view_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))

          #-------- fonction pour afficher une image dans l admin partie des inline-----------------#

      def detail_image(self,obj):

          return mark_safe('<img src="{img_url}" width="100px", heigth="100px"/>'.format(img_url=obj.image.url))

      #-------------------- Permet de Créer un search box dans la page de listing------------------#
      search_fields = ('nom',)
      date_hierarchy = 'date_add'
      #---------pour personnaliser les actions de l admin-------------#
      actions = (
          'active',
          'desactive',
      )
      #--------Pour rendre un element cliquable. tous les elements sauf standards ----------------#
      list_display_links = [
          'view_image',
          'nom',
          ]

      #--------------Pour afficher le nombre d element par Page---------------------#
      list_per_page = 3

      #------------- Ordonner par ordre alphabetique ---------------#
      ordering = [
          'nom',
      ]
      #----------------------- il permet d afficher l image dans l admin cote tabulariline-----------#
      readonly_fields = ['detail_image']

      #----------------------- Definition des fonctions pour gerer les actions-----------#
      def active(self,request,queryset):
          queryset.update(status=True)
          self.message_user(request,"la selection a ete activer avec succes")
      active.short_description = "Activer les Categorie selectionner"

      def desactive(self,request,queryset):
          queryset.update(status=False)
          self.message_user(request,"la selection a ete desactiver avec succes")
      desactive.short_description = "desactiver les Categorie selectionner"
  ```
