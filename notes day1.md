Some of these applications make use of at least one database table, though, so we need to create the tables in the database before we can use them. To do that, run the following command:

$ python manage.py migrate

The migrate command looks at the INSTALLED_APPS setting and creates any necessary database tables according to the database settings in your mysite/settings.py file and the database migrations shipped with the app (we’ll cover those later). 


To include the app in our project, we need to add a reference to its configuration class in the INSTALLED_APPS setting. The PollsConfig class is in the polls/apps.py file, so its dotted path is 'polls.apps.PollsConfig'.
Edit the mysite/settings.py file and add that dotted path to the INSTALLED_APPS setting. 
That (INSTALLED_APPS) holds the names of all Django applications that are activated in this Django instance. Apps can be used in multiple projects, and you can package and distribute them for use by others in their projects.


Now Django knows to include the polls app. Let’s run another command:
$ python manage.py makemigrations polls

By running makemigrations, you’re telling Django that you’ve made some changes to your models (in this case, you’ve made new ones) and that you’d like the changes to be stored as a migration.

Migrations are how Django stores changes to your models (and thus your database schema) - they’re files on disk.
There’s a command that will run the migrations for you and manage your database schema automatically - that’s called migrate, and we’ll come to it in a moment - but first, let’s see what SQL that migration would run. The sqlmigrate command takes migration names and returns their SQL:
python manage.py sqlmigrate polls 0001

    the migrate command takes all the migrations that haven’t been applied (Django tracks which ones are applied using a special table in your database called django_migrations) and runs them against your database - essentially, synchronizing the changes you made to your models with the schema in the database.

Migrations are very powerful and let you change your models over time, as you develop your project, without the need to delete your database or tables and make new ones - it specializes in upgrading your database live, without losing data. We’ll cover them in more depth in a later part of the tutorial, but for now, remember the three-step guide to making model changes:

Change your models (in models.py).

Run python manage.py makemigrations to create migrations for those changes.
q   
Run python manage.py migrate to apply those changes to the database.

The reason that there are separate commands to make and apply migrations is because you’ll commit migrations to your version control system and ship them with your app; they not only make your development easier, they’re also usable by other developers and in production.


MODELS:
A model is the single, definitive source of information about your data. It contains the essential fields and behaviors of the data you’re storing. Django follows the DRY Principle. The goal is to define your data model in one place and automatically derive things from it.
This includes the migrations, which are derived from your models file. They form a history that Django uses to update your database schema to match your current models.

In polls app we will create two models:
QUESTION
CHOICE

a question has a question and a publication date.
a choice has two fields: the text of the choice and vote tally, each choice is associated with a question.

 Django apps are “pluggable”: You can use an app in multiple projects, and you can distribute apps, because they don’t have to be tied to a given Django installation.