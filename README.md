# query-language

## Idea

```ql

{
  @point user-frineds with users_table { # generate to GET /user-friends endpoint
    @middleware service::auth>has(id)
    @middleware service::auth->reset_login_cache(self->ifdef)
    id as object_id(id)
    name as userName
    last_name as userLastName
    time_created as join_date
    user_information.email as age
    user_contact.email as email
    @joinArray @table->user_information self.id -> @point.id & self.active = true
    @join @table->user_contact self.id -> @point.id & self.active = true
    @where active=true
  }
  # -> with Parameeters
  @point user-frineds with users_table (par_age,par_email) as getUserFrineds { # generate to GET /user-friends endpoint
    id as object_id(id)
    name as userName
    last_name as userLastName
    time_created as join_date
    user_information.email as age
    user_contact.email as email
    @joinArray @table->user_information self.id -> @point.id & self.active = true & self.age=par_age
    @join @table->user_contact self.id -> @point.id & self.active = true & self.email=par_email
    @where active=true 
  }
  @point users with @useView(getUserFrineds)->as(friendList) (par_age,par_email) { # generate to GET /user-friends endpoint
    id as object_id(id)
    name as userName
    last_name as userLastName
    time_created as join_date
    @get(friendList) as friend_list
    @where active=true 
  }
  @CQuery get_users with users { # generate to GET /user-friends endpoint
    id as object_id(id)
    name as userName
    last_name as userLastName
    time_created as join_date
    @where active=true 
  }
}

```
