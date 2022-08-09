# TheWantedMan
Play as an inmate, trying to escape from jail to the safehouse.
```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;

namespace TheWantedMan_Game__Unit14_MunasarMuhidin
{
    public partial class Form1 : Form
    {
        public Form1()
        {
            InitializeComponent();
        }

        //declarations
        //declare moveLeft as bool
        bool moveLeft;
        //declare moveRight as bool
        bool moveRight;
        //declare moveUp as bool
        bool moveUp;
        //declare moveDown as bool
        bool moveDown;
        bool beenCaught = false;
        int currentFrame = 1;
        int position = 0;

        bool wallCollisions(int left, int right, int top, int bottom)
        {
            //code for collision detection
            PictureBox[] treelogs = { treelog, treelog2, trees,};
  
            int numWalls = treelogs.Length;

            for (int i = 0; i < numWalls; i++)
            {
                if ((left < treelogs[i].Right) &&
                    (right > treelogs[i].Left) &&
                    (top < treelogs[i].Bottom) &&
                    (bottom > treelogs[i].Top) &&
                    treelogs[i].Visible == true)
                {
                    return true;
                }
            }
            PictureBox[] policeMen = { police, police2 };
            return false;
        }

        private void frameChange()
        {
            PictureBox[] pics = { trees, treelog, treelog2 };
            PictureBox[] hardPics = {helicopter, bossPolice};
            int i = 0;
            int m = 0;

            //frame1 -> frame 2
            if(currentFrame == 1 && player.Top <= 0)
            {
                player.Top = this.Height - 125;
                currentFrame = 2;

                for (i = 0; i <= pics.Length - 1; i++)
                {
                    pics[i].Visible = true;


                }

            }



            //frame2 -> frame3
            if (currentFrame == 2 && player.Top <= 0)
            {
                player.Top = this.Height - 125;
                currentFrame = 3;

                for (i = 0; i <= pics.Length - 1; i++)
                {
                    pics[i].Visible = false;
                    
                }

                for(m=0;m<=hardPics.Length-1;m++)
                {
                   hardPics[m].Visible = false;
                    police3.Visible = true;
                }

            }

           
            
            //frame3 -> frame4
            if(currentFrame == 3 && player.Top <= 0)
            {
                player.Top = this.Height - 125;
                currentFrame = 4;
                police.Visible = false;
                police2.Visible = false;
                police3.Visible = false;

               

                for(i=0;i<=hardPics.Length - 1;i++)
                {
                    hardPics[i].Visible = true;

                    Random n = new Random();
                    position = n.Next(0, 2);
                    

                    if(position == 1)
                    {
                        bossPolice.Left = this.Width - 200;
                    }

                }
            }
            
        }

        private void movement_Tick(object sender, EventArgs e)
        {

            if (player.Top <= 10 && currentFrame == 4)
            {
                movement.Enabled = false;
                bigPoliceMovement.Enabled = false;
                policeHeli.Enabled = false;
                MessageBox.Show("Well done you have finished the game!");
            }


            //code for player colliding with the treelogs
            if ((moveLeft == true) && !wallCollisions(player.Left, player.Right - 3, player.Top, player.Bottom))
            {
                player.Left = player.Left - 3;
                player.Image = playerLeft.Image;
            }

            if ((moveRight == true) && !wallCollisions(player.Left + 3, player.Right, player.Top, player.Bottom))
            {
                player.Left = player.Left + 3;
                player.Image = playerRight.Image;
            }

            if ((moveUp == true) && !wallCollisions(player.Left + 3, player.Right - 3, player.Top - 3, player.Bottom))
            {
                player.Top = player.Top - 3;
                player.Image = playerUp.Image;
                frameChange();
            }

            if ((moveDown == true) && !wallCollisions(player.Left + 3, player.Right - 3, player.Top, player.Bottom + 3))
            {
                player.Top = player.Top + 3;
                player.Image = playerDown.Image;
            }

            foreach (Control picBox in this.Controls)
            {
                if (picBox is PictureBox && picBox.Tag == "boundariesBottom")
                {
                    if (player.Bounds.IntersectsWith(picBox.Bounds))
                    {
                        player.Top = picBox.Top - player.Height;
                    }
                }
                
            }

        }

        private void Form1_KeyDown(object sender, KeyEventArgs e)
        {
          //code for the keys, WASD
          if (e.KeyCode == Keys.A)
          {
                moveLeft = true;
          }

          if (e.KeyCode == Keys.W)
          {
                moveUp = true;
          }

          if (e.KeyCode == Keys.D)
          {
                moveRight = true;
          }

          if (e.KeyCode == Keys.S)
          {
                moveDown = true;
          }
        }

        private void Form1_KeyUp(object sender, KeyEventArgs e)
        {
            //code for the keys, WASD
            if (e.KeyCode == Keys.A)
            {
                moveLeft = false;
            }

            if (e.KeyCode == Keys.W)
            {
                moveUp = false;
            }

            if (e.KeyCode == Keys.D)
            {
                moveRight = false;
            }

            if (e.KeyCode == Keys.S)
            {
                moveDown = false;
            }
        }

        private void Form1_Load(object sender, EventArgs e)
        {

        }

        bool moveOpposite = true;
        private void police1Movement_Tick(object sender, EventArgs e)
        {
            if(police.Right >= (this.Width-10))
            {
                moveOpposite = true;
                police.Image = policeLeft.Image;
            }

            if (police.Left <= 309)
            {
                moveOpposite = false;
                police.Image = policeRight.Image;
            }

            if(moveOpposite == true)
            {
                police.Left = police.Left - 10;
            }
            else
            {
                police.Left = police.Left + 10;
            }
        }

        bool moveOther = true;
        private void police2Movement_Tick(object sender, EventArgs e)
        {
            if(police2.Bottom >= this.Height-20)
            {
                moveOther = true;
                police2.Image = policeUp.Image;
            }

            if (police2.Top <= 124)
            {
                moveOther = false;
                police2.Image = policeDown.Image;
            }

            if (moveOther == true)
            {
                police2.Top = police2.Top - 10;
            }
            else
            {
                police2.Top = police2.Top + 10;
            }

        }

        private void gameOver_Tick(object sender, EventArgs e)
        {
            foreach (Control picBox in this.Controls)
            {
                if (picBox is PictureBox && picBox.Tag == "police")
                {
                    if (player.Bounds.IntersectsWith(picBox.Bounds) && picBox.Visible == true)
                    {
                        caught();
                        
                    }
                }
            }
        }

       

        int time = 0;
        private void playTime_Tick(object sender, EventArgs e)
        {
            score.Text = "Time: " + time.ToString();
            time++;
        }


        string moveHeli = "right";
        private void policeHeli_Tick(object sender, EventArgs e)
        {
            if(helicopter.Top >= 10 && moveHeli == "up")
            {
                helicopter.Top -= 10;

                if (helicopter.Top <= 10)
                {
                    moveHeli = "right";
                }
            }



            if (helicopter.Right >= 10 && moveHeli == "right")
            {
                helicopter.Left += 10;

                if (helicopter.Top <= this.Width-10)
                {
                    moveHeli = "down";
                }
            }

            if (helicopter.Bottom <= this.Height-50 && moveHeli == "down")
            {
                helicopter.Top += 10;

                if (helicopter.Bottom >= this.Height - 100)
                {
                    moveHeli = "left";
                }
            }

            if (helicopter.Left > 10 && moveHeli == "left")
            {
                helicopter.Left -= 10;

                if (helicopter.Left <= 10)
                {
                    moveHeli = "up";
                }
            }

        }

        private void caught()
        {
            gameOver.Enabled = false;
            movement.Enabled = false;
            policeHeli.Enabled = false;
            bigPoliceMovement.Enabled = false;
            police1Movement.Enabled = false;
            police2Movement.Enabled = false;
            police3Movement.Enabled = false;
            playTime.Stop();
            MessageBox.Show("Game Over." + "\n" + "Score: " + time.ToString());

        }

        bool movePolice = true;

        private void police3Movement_Tick(object sender, EventArgs e)
        {
            if (police3.Bottom >= this.Height - 100)
            {
                movePolice = true;
                police3.Image = policeUp.Image;
            }

            if (police3.Top <= 100)
            {
                movePolice = false;
                police3.Image = policeDown.Image;
            }

            if (movePolice == true)
            {
                police3.Top = police3.Top - 10;
            }
            else
            {
                police3.Top = police3.Top + 10;
            }
        }

        bool movebigPolice = true;
        private void bigPoliceMovement_Tick(object sender, EventArgs e)
        {
           
            if (position == 0)
            { 
                if (bossPolice.Right >= this.Width - 100)
                {
                    movebigPolice = true;
                    bossPolice.Image = bossPolice.Image;
                }

                if (bossPolice.Left <= 100)
                {
                    movebigPolice = false;
                    bossPolice.Image = bossPolice.Image;
                }

                if (movebigPolice == true)
                {
                    bossPolice.Left = bossPolice.Left - 10;
                }
                else
                {
                    bossPolice.Left = bossPolice.Left + 10;
                }
            }

            if (position == 1)
            {
                if (bossPolice.Bottom >= this.Height - 100)
                {
                    movebigPolice = true;
                    bossPolice.Image = bossPolice.Image;
                }

                if (bossPolice.Top <= 100)
                {
                    movebigPolice = false;
                    bossPolice.Image = bossPolice.Image;
                }

                if (movebigPolice == true)
                {
                    bossPolice.Top = bossPolice.Top - 10;
                }
                else
                {
                    bossPolice.Top = bossPolice.Top + 10;
                }
            }

        }
    }
}
```
