//Juan Ruiz
//CAPI Homework #7 (CAPI System Call HW #7)
//November 10, 2017

#include <stdio.h>
#include <string.h>
#include <ctype.h>


#define GAME_RANK   25


struct rank_type
{
   char Title[128];
   char ReviewDate[128];
   char IGNRating[128];
};


char  *ptr, buffer[16392], cmd[512];
//char  company_name[128], zipcode[64];
//char  wind_speed[64], wind_direction[64];
//char  us_state[32],
 us_city[128], tod[64];
int   i, n, first_time, indx, state;
struct rank_type the_rating[GAME_RANK];

int main(int argc, char *argv[])
{
   FILE *fp;

   sprintf(cmd,
      "wget -qO- http://www.ign.com/reviews/games");

   if((fp = popen(cmd, "r")) != NULL)
   {
      state = 0;

      while (!feof(fp))
      {
         if (fgets(buffer, sizeof(buffer) - 1, fp) != NULL)
         {

           //Debugging Code
           //sleep(1);
           //printf( "DEBUG:  %s\n", buffer);

           switch (state)
           {
              case 0:
                 if (strstr(buffer, "<br><table border") != NULL)
                 {
                    i = 0;
                    state = 1;
                 }
              break;

              case 1:
                 if (strstr(buffer, "the_rating") != NULL)
                 {

                    if ((ptr = strstr(buffer, "<b>")) != NULL)
                    {
                       ptr += 3;

                       indx = 0;
                       while (*ptr != '<')
                       {
                          if ((*ptr == '&') && (*(ptr+1) == 'a') && (*(ptr+2) ==                                                                                         'm') && (*(ptr+3) == 'p'))
                          {
                             ptr+=4;
                             the_rating[i].name[indx] = '&';
                          }
                          else
                          {
                             the_rating[i].name[indx] = *ptr;
                          }

                          indx++;
                          ptr++;
                       }

                       state = 2;
                    }
                 }
              break;

              case 2:
                if (strstr(buffer, "studio") != NULL)
                {
                    if ((ptr = strstr(buffer, ".htm\">")) != NULL)
                    {
                        ptr += 6;
                        indx = 0;
                      //Additional Studio Information
                        while (*ptr != '<')
                        {
                           the_rating[i].studio[indx] = *ptr;
                           indx++;
                           ptr++;
                        }
                        state = 3;
                        /*if(++i >= GAME_RANK)
                          state = 3; //4
                        else
                          state = 1; */

                    }
                }

              break;

              case 3:
                 if ((ptr = strstr(buffer, "right")) != NULL)
                 {
                    if ((ptr = strstr(buffer, "<b>")) != NULL)
                    {
                       ptr += 3;

                       indx = 0;
                       //Additional Gross Information
                       while (*ptr != '<')
                       {
                          the_rating[i].gross[indx] = *ptr;

                          indx++;
                          ptr++;
                       }
                       state = 4;
                        /* if (++i >= GAME_RANK)
                            state = 4; //3
                          else
                            state = 1; */

                    }
                 }


              break;

              case 4:
                state = 5;
              break;

              case 5:
                if ((ptr = strstr(buffer, "right")) != NULL)
                 {
                    if ((ptr = strstr(buffer, "2\">")) != NULL)
                    {
                       ptr += 3;

                       indx = 0;
                      //Additional Information For Number Of Theaters
                       while (*ptr != '<')
                       {
                          the_rating[i].numOfTheaters[indx] = *ptr;

                          indx++;
                          ptr++;
                       }
                    }
                 }
                    if (++i >= GAME_RANK)
                       {
                          state = 6;
                       }
                       else
                          state = 1;

              break;
            }
         }
      }

      pclose(fp);

      // display results

      for (i=0,first_time=1;i < GAME_RANK;i++)
      {
         if (strlen(the_rating[i].name) != 0)
         {
            if (first_time)
            {
               printf("\n               ------------ TOP 30 Weekend the_rating -----                                                                                        -------\n");
               printf("\n----------------the_rating------------------Gross---------S                                                                                        tudio-------Number Of Theaters\n");

               first_time = 0;
            }

            printf("  %2d) %-28.28s   %-15.15s    %-10.10s  %s\n",
                i+1,
                the_rating[i].name,
                the_rating[i].gross,
                the_rating[i].studio,
                the_rating[i].numOfTheaters);
         }
      }
   }

   return 0;
}
