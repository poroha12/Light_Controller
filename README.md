using System;
using System.Collections.Generic;
using System.Collections;
using System.ComponentModel;
using System.Drawing;
using System.Data;
using System.Runtime.InteropServices;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Threading;
using System.IO;
using System.IO.Ports;


namespace LightingControllerApp
{
    public partial class Form1 : Form
    {
        string str;

        //포트 설정
        //private SerialPort1 SerialPort1 = new SerialPort1();
        //int readCnt = 0;
        //byte recvByte = 0;
        //byte[] recvBuf = new byte[1024];

        public Form1()
        {
            InitializeComponent();
       
            string[] PortNames = SerialPort.GetPortNames(); //포트 검색
            foreach (string portnumber in PortNames)
            {
                comboBox.Items.Add(portnumber);          // 검색한 포트를 콤보박스에 입력. 
            }
        }

        //private void Form1_Load(object sender, EventArgs e)
        //{
        //}

        private void btnDisconnect_Click(object sender, EventArgs e)
        {
            if (SerialPort1.IsOpen)
            {
                SerialPort1.Close();
                textBox7.Text += "해제되었습니다" + Environment.NewLine;
            }
            else
            {
                textBox7.Text += "해제되어 있습니다." + Environment.NewLine;
            }

            //if (!(Convert.ToInt32(textBox1.Text) < 0 || Convert.ToInt32(textBox1.Text) > 255
            //    || Convert.ToInt32(textBox2.Text) < 0 || Convert.ToInt32(textBox2.Text) > 255
            //    || Convert.ToInt32(textBox3.Text) < 0 || Convert.ToInt32(textBox3.Text) > 255
            //    || Convert.ToInt32(textBox4.Text) < 0 || Convert.ToInt32(textBox4.Text) > 255
            //    || Convert.ToInt32(textBox5.Text) < 0 || Convert.ToInt32(textBox5.Text) > 255
            //    || Convert.ToInt32(textBox6.Text) < 0 || Convert.ToInt32(textBox6.Text) > 255))
            //{
            //    byte A1 = Convert.ToByte(textBox1.Text);
            //    byte A2 = Convert.ToByte(textBox2.Text);
            //    byte A3 = Convert.ToByte(textBox3.Text);
            //    byte A4 = Convert.ToByte(textBox4.Text);
            //    byte A5 = Convert.ToByte(textBox5.Text);
            //    byte A6 = Convert.ToByte(textBox6.Text);

            //    byte[] AllSend = new byte[12];

            //    AllSend[0] = 0xEF;
            //    AllSend[1] = 0xEF;
            //    AllSend[2] = 0x00;

            //    AllSend[3] = A1;
            //    AllSend[4] = A2;
            //    AllSend[5] = A3;
            //    AllSend[6] = A4;
            //    AllSend[7] = A5;
            //    AllSend[8] = A6;

            //    int test = AllSend[2] ^ AllSend[3] ^ AllSend[4] ^ AllSend[5] ^ AllSend[6] ^ AllSend[7] ^ (AllSend[8] + 0x01);

            //    AllSend[9] = (byte)test;
            //    AllSend[10] = 0xEE;
            //    AllSend[11] = 0xEE;

            //    SerialPort1.DiscardInBuffer();
            //    SerialPort1.Write(AllSend, 0, 12);
            //    textBox7.Text += "전송 완료" + Environment.NewLine;
            //}
            //else
            //{
            //    MessageBox.Show("0~255사이의 값을 입력해 주세요");
            //}
        }

        private void btnConnect_Click(object sender, EventArgs e)
        {
            if (!SerialPort1.IsOpen) //닫혀있을때
            {
                SerialPort1.PortName = comboBox.Text; //콤보박스에서 고른 것을 포트네임으로 넣어준다
                SerialPort1.BaudRate = 9600; //콤보박스에서 고른 것(string)을 int로 변경해서 넣어준다.
                SerialPort1.DataBits = 8; // 8비트 데이터 전송은 고정
                SerialPort1.StopBits = StopBits.One; // stop비트는 1로 고정
                SerialPort1.Parity = Parity.None; // 패리티비트는 없는 걸로
                SerialPort1.DataReceived += new SerialDataReceivedEventHandler(SerialPort1_DataReceived); //데이터 받기
                SerialPort1.Open();
                textBox7.Text += "연결댐" + Environment.NewLine;

                comboBox.Enabled = false;

                // 조명 컨트롤러에 연결
                //if (!SerialPort1.IsOpen)
                //    {
                //        SerialPort1.PortName = comboBox.Text;
                //        SerialPort1.Open();

                //        textBox7.Text += "Port ON Success" + Environment.NewLine;
                //    }

                //    // 연결 상태에 따라 UI 업데이트
                //    if (SerialPort1.IsOpen)
                //    {
                //        btnConnect.Enabled = false;
                //        btnTurnOnAll.Enabled = true;
                //        btnTurnOffAll.Enabled = true;
                //        this.BackColor = System.Drawing.Color.White; // 기본 배경색
                //    }

            }

            else
            {
                //SerialPort1.Close();
                textBox7.Text += "이미 연결 댐" + Environment.NewLine;
            }

        }

        private void SerialPort1_DataReceived(object sender, SerialDataReceivedEventArgs e)
        {
            str = SerialPort1.ReadExisting();

            if (str.Length > 0)
            {
                textBox7.SelectedText += str + Environment.NewLine;
            }
        }



        private void comboBox_SelectedIndexChanged(object sender, EventArgs e)
        {

        }
        private void Databits_Combox_SelectedIndexChanged(object sender, EventArgs e)
        {

        }

        private void Stopbits_Combox_SelectedIndexChanged(object sender, EventArgs e)
        {

        }

        private void Parity_Combox_SelectedIndexChanged(object sender, EventArgs e)
        {

        }

        private void textBox7_TextChanged(object sender, EventArgs e)
        {
            textBox7.SelectionStart = textBox7.TextLength;//스크롤 자동으로 내린다.
            textBox7.ScrollToCaret();

        }
        //private void btnDisconnect_Click(object sender, EventArgs e)
        //{
        //    // 조명 컨트롤러와 연결 해제
        //    if (lightingController.IsOpen)
        //        lightingController.Close();

        //    // 연결 상태에 따라 UI 업데이트
        //    if (!lightingController.IsOpen)
        //    {
        //        btnConnect.Enabled = true;
        //        btnDisconnect.Enabled = false;
        //        btnTurnOnAll.Enabled = false;
        //        btnTurnOffAll.Enabled = false;
        //        this.BackColor = System.Drawing.Color.Gray; // 연결 해제시 배경색 변경
        //    }
        //}


        private void textBox1_KeyPress(object sender, KeyPressEventArgs e)
        {
            if (!(char.IsDigit(e.KeyChar) || e.KeyChar == Convert.ToChar(Keys.Back) || e.KeyChar == Convert.ToChar(Keys.Escape)))    //숫자와 백스페이스를 제외한 나머지를 바로 처리
            {
                e.Handled = true;
                MessageBox.Show("0~255의 숫자만 입력하세요.");
            }
        }

        private void btnTurnOnAll_Click(object sender, EventArgs e)
        {
            // 모든 조명 켜기
            //if (SerialPort1.IsOpen)
            //{
            //    // 조명 컨트롤러에 켜기 명령 전송
            //    SerialPort1.Write("ON");

            //    // UI 업데이트
            //    btnTurnOnAll.BackColor = System.Drawing.Color.Yellow;
            //}
        }
        private void btnTurnOffAll_Click(object sender, EventArgs e)
        {
            // 모든 조명 끄기
            //if (SerialPort1.IsOpen)
            //{
            //    // 조명 컨트롤러에 끄기 명령 전송
            //    SerialPort1.Write("OFF");

            //    // UI 업데이트
            //    btnTurnOnAll.BackColor = System.Drawing.SystemColors.Control;
            //}
        }

        // while문으로 led가 계속 onoff될 수 있도록 loop 작성
        //private void loop()
        //{
        //    if (!(Convert.ToInt32(textBox1.Text) < 0 || Convert.ToInt32(textBox1.Text) > 255
        //        || Convert.ToInt32(textBox2.Text) < 0 || Convert.ToInt32(textBox2.Text) > 255
        //        || Convert.ToInt32(textBox3.Text) < 0 || Convert.ToInt32(textBox3.Text) > 255
        //        || Convert.ToInt32(textBox4.Text) < 0 || Convert.ToInt32(textBox4.Text) > 255
        //        || Convert.ToInt32(textBox5.Text) < 0 || Convert.ToInt32(textBox5.Text) > 255
        //        || Convert.ToInt32(textBox6.Text) < 0 || Convert.ToInt32(textBox6.Text) > 255))

        //    {
        //        byte A1 = Convert.ToByte(textBox1.Text);
        //        byte A2 = Convert.ToByte(textBox2.Text);
        //        byte A3 = Convert.ToByte(textBox3.Text);
        //        byte A4 = Convert.ToByte(textBox4.Text);
        //        byte A5 = Convert.ToByte(textBox5.Text);
        //        byte A6 = Convert.ToByte(textBox6.Text);

        //        byte[] AllSend = new byte[12];

        //        AllSend[0] = 0xEF;
        //        AllSend[1] = 0xEF;
        //        AllSend[2] = 0x00;

        //        AllSend[3] = A1;
        //        AllSend[4] = A2;
        //        AllSend[5] = A3;
        //        AllSend[6] = A4;
        //        AllSend[7] = A5;
        //        AllSend[8] = A6;

        //        int test = AllSend[2] ^ AllSend[3] ^ AllSend[4] ^ AllSend[5] ^ AllSend[6] ^ AllSend[7] ^ (AllSend[8] + 0x01);

        //        AllSend[9] = (byte)test;
        //        AllSend[10] = 0xEE;
        //        AllSend[11] = 0xEE;


        //        byte[] AllSend2 = new byte[12];

        //        AllSend2[0] = 0xEF;
        //        AllSend2[1] = 0xEF;
        //        AllSend2[2] = 0x00;

        //        AllSend2[3] = 0x00;
        //        AllSend2[4] = 0x00;
        //        AllSend2[5] = 0x00;
        //        AllSend2[6] = 0x00;
        //        AllSend2[7] = 0x00;
        //        AllSend2[8] = 0x00;


        //        int test1 = AllSend2[2] ^ AllSend2[3] ^ AllSend2[4] ^ AllSend2[5] ^ AllSend2[6] ^ AllSend2[7] ^ (AllSend2[8] + 0x01);

        //        AllSend2[9] = (byte)test1;
        //        AllSend2[10] = 0xEE;
        //        AllSend2[11] = 0xEE;

        //        while (loops.IsAlive)
        //        {
        //            SerialPort1.Write(AllSend, 0, 12);
        //            System.Threading.Thread.Sleep(1000);

        //            SerialPort1.Write(AllSend2, 0, 12);
        //            System.Threading.Thread.Sleep(1000);
        //        }

        //    }
        //    else
        //    {
        //        MessageBox.Show("0~255사이의 값을 입력해 주세요");
        //    }
        //}

        //bool state = false;

        //private Thread loops = null;

        // loop를 활용해 쓰레드를 생성하고 실행

        //private void ChOnOff_Click(object sender, EventArgs e)
        //{
        //    state = !state;

        //    this.loops = new Thread(new ThreadStart(loop));

        //    if (state == true)
        //    {
        //        try
        //        {
        //            loops.Start();
        //        }
        //        catch (Exception er)
        //        {
        //            MessageBox.Show(er.Message, "Error", MessageBoxButtons.OK, MessageBoxIcon.Error);
        //        }
        //        textBox7.Text += "Light Start" + Environment.NewLine;
        //    }
        //    else
        //    {
        //        try
        //        {
        //            loops.Abort();

        //            textBox7.Text += "Light Stop" + Environment.NewLine;
        //        }
        //        catch (Exception)
        //        {

        //        }
        //    }
        //    loops.IsBackground = true;
        //}

        // 1~8번이 돌아가면서 실행 될 수 있도록 루프값 생성
        //private void loop2()
        //{
        //    if (!(Convert.ToInt32(textBox1.Text) < 0 || Convert.ToInt32(textBox1.Text) > 255
        //        || Convert.ToInt32(textBox2.Text) < 0 || Convert.ToInt32(textBox2.Text) > 255
        //        || Convert.ToInt32(textBox3.Text) < 0 || Convert.ToInt32(textBox3.Text) > 255
        //        || Convert.ToInt32(textBox4.Text) < 0 || Convert.ToInt32(textBox4.Text) > 255
        //        || Convert.ToInt32(textBox5.Text) < 0 || Convert.ToInt32(textBox5.Text) > 255
        //        || Convert.ToInt32(textBox6.Text) < 0 || Convert.ToInt32(textBox6.Text) > 255))

        //    {
        //        byte A1 = Convert.ToByte(textBox1.Text);
        //        byte A2 = Convert.ToByte(textBox2.Text);
        //        byte A3 = Convert.ToByte(textBox3.Text);
        //        byte A4 = Convert.ToByte(textBox4.Text);
        //        byte A5 = Convert.ToByte(textBox5.Text);
        //        byte A6 = Convert.ToByte(textBox6.Text);

        //        byte[] AllSend = new byte[12];

        //        AllSend[0] = 0xEF;
        //        AllSend[1] = 0xEF;
        //        AllSend[2] = 0x00;

        //        AllSend[3] = A1;
        //        AllSend[4] = 0x00;
        //        AllSend[5] = 0x00;
        //        AllSend[6] = 0x00;
        //        AllSend[7] = 0x00;
        //        AllSend[8] = 0x00;

        //        int test = AllSend[2] ^ AllSend[3] ^ AllSend[4] ^ AllSend[5] ^ AllSend[6] ^ AllSend[7] ^ (AllSend[8] + 0x01);

        //        AllSend[9] = (byte)test;
        //        AllSend[10] = 0xEE;
        //        AllSend[11] = 0xEE;



        //        byte[] AllSend2 = new byte[12];

        //        AllSend2[0] = 0xEF;
        //        AllSend2[1] = 0xEF;
        //        AllSend2[2] = 0x00;

        //        AllSend2[3] = 0x00;
        //        AllSend2[4] = A1;
        //        AllSend2[5] = 0x00;
        //        AllSend2[6] = 0x00;
        //        AllSend2[7] = 0x00;
        //        AllSend2[8] = 0x00;

        //        int test1 = AllSend2[2] ^ AllSend2[3] ^ AllSend2[4] ^ AllSend2[5] ^ AllSend2[6] ^ AllSend2[7] ^ (AllSend2[8] + 0x01);

        //        AllSend2[9] = (byte)test1;
        //        AllSend2[10] = 0xEE;
        //        AllSend2[11] = 0xEE;

        //        byte[] AllSend3 = new byte[12];

        //        AllSend3[0] = 0xEF;
        //        AllSend3[1] = 0xEF;
        //        AllSend3[2] = 0x00;

        //        AllSend3[3] = 0x00;
        //        AllSend3[4] = 0x00;
        //        AllSend3[5] = A1;
        //        AllSend3[6] = 0x00;
        //        AllSend3[7] = 0x00;
        //        AllSend3[8] = 0x00;


        //        int test2 = AllSend3[2] ^ AllSend3[3] ^ AllSend3[4] ^ AllSend3[5] ^ AllSend3[6] ^ AllSend3[7] ^ (AllSend3[8] + 0x01);

        //        AllSend3[9] = (byte)test1;
        //        AllSend3[10] = 0xEE;
        //        AllSend3[11] = 0xEE;

        //        byte[] AllSend4 = new byte[12];

        //        AllSend4[0] = 0xEF;
        //        AllSend4[1] = 0xEF;
        //        AllSend4[2] = 0x00;

        //        AllSend4[3] = 0x00;
        //        AllSend4[4] = 0x00;
        //        AllSend4[5] = 0x00;
        //        AllSend4[6] = A1;
        //        AllSend4[7] = 0x00;
        //        AllSend4[8] = 0x00;

        //        int test3 = AllSend4[2] ^ AllSend4[3] ^ AllSend4[4] ^ AllSend4[5] ^ AllSend4[6] ^ AllSend3[7] ^ (AllSend3[8] + 0x01);

        //        AllSend4[9] = (byte)test1;
        //        AllSend4[10] = 0xEE;
        //        AllSend4[11] = 0xEE;

        //        byte[] AllSend5 = new byte[12];

        //        AllSend5[0] = 0xEF;
        //        AllSend5[1] = 0xEF;
        //        AllSend5[2] = 0x00;

        //        AllSend5[3] = 0x00;
        //        AllSend5[4] = 0x00;
        //        AllSend5[5] = 0x00;
        //        AllSend5[6] = 0x00;
        //        AllSend5[7] = A1;
        //        AllSend5[8] = 0x00;

        //        int test4 = AllSend5[2] ^ AllSend5[3] ^ AllSend5[4] ^ AllSend5[5] ^ AllSend5[6] ^ AllSend3[7] ^ (AllSend3[8] + 0x01);

        //        AllSend5[9] = (byte)test1;
        //        AllSend5[10] = 0xEE;
        //        AllSend5[11] = 0xEE;

        //        byte[] AllSend6 = new byte[12];

        //        AllSend6[0] = 0xEF;
        //        AllSend6[1] = 0xEF;
        //        AllSend6[2] = 0x00;

        //        AllSend6[3] = 0x00;
        //        AllSend6[4] = 0x00;
        //        AllSend6[5] = 0x00;
        //        AllSend6[6] = 0x00;
        //        AllSend6[7] = 0x00;
        //        AllSend6[8] = A1;

        //        int test5 = AllSend6[2] ^ AllSend6[3] ^ AllSend6[4] ^ AllSend6[5] ^ AllSend6[6] ^ AllSend3[7] ^ (AllSend3[8] + 0x01);

        //        AllSend6[9] = (byte)test1;
        //        AllSend6[10] = 0xEE;
        //        AllSend6[11] = 0xEE;


        //        try
        //        {


        //            while (loops2.IsAlive)
        //            {
        //                SerialPort1.Write(AllSend, 0, 12);
        //                System.Threading.Thread.Sleep(1000);

        //                SerialPort1.Write(AllSend2, 0, 12);
        //                System.Threading.Thread.Sleep(1000);

        //                SerialPort1.Write(AllSend3, 0, 12);
        //                System.Threading.Thread.Sleep(1000);

        //                SerialPort1.Write(AllSend4, 0, 12);
        //                System.Threading.Thread.Sleep(1000);

        //                SerialPort1.Write(AllSend5, 0, 12);
        //                System.Threading.Thread.Sleep(1000);

        //                SerialPort1.Write(AllSend6, 0, 12);
        //                System.Threading.Thread.Sleep(1000);

        //            }

        //        }
        //        catch (Exception e) { }

        //    }
        //    else
        //    {
        //        MessageBox.Show("0~255사이의 값을 입력해 주세요");
        //    }
        //}

        //bool state2 = false;

        //private Thread loops2 = null;

        //private void LC_data(object sender, SerialErrorReceivedEventArgs e)
        //{
        //    try
        //    {
        //        SerialPort myport = (SerialPort)sender;
        //        string s = myport.ReadExisting();

        //        int Ardbtn = Convert.ToInt32(s);

        //        state = !state;
        //        state2 = !state2;

        //        //this.loops = new Thread(new ThreadStart(this.loop));
        //        //this.loops2 = new Thread(new ThreadStart(this.loop2));

        //        switch (Ardbtn)
        //        {
        //            case 1:
        //                loops.Start();
        //                textBox7.Text += " Light Start" + Environment.NewLine;
        //                break;
        //            case 2:
        //                loops.Abort();
        //                loops2.Abort();
        //                textBox7.Text += " Light Stop" + Environment.NewLine;
        //                loops.IsBackground = true;
        //                loops2.IsBackground = true;
        //                break;
        //            case 3:
        //                loops2.Start();
        //                textBox7.Text += " Light Start-2" + Environment.NewLine;
        //                break;
        //        }
        //    }
        //    catch (Exception eee)
        //    {

        //    }

        //}

        private void listClear_Click(object sender, EventArgs e)
        {
            textBox7.Clear();
        }

        private void Form1_FormClosing(object sender, FormClosingEventArgs e)
        {
            if (MessageBox.Show("종료 하시겠습니까?", "종료",
                MessageBoxButtons.YesNo, MessageBoxIcon.Question,
                MessageBoxDefaultButton.Button1) == DialogResult.No)
            {
                e.Cancel = true;
            }
        }

        public void btnExit_Click(object sender, EventArgs e)
        {
            // 종료 확인 메시지 표시
            DialogResult result = MessageBox.Show("프로그램을 종료하시겠습니까?", "종료", MessageBoxButtons.YesNo, MessageBoxIcon.Question);

            // 사용자가 "아니오"를 선택한 경우 종료 취소
            if (result == DialogResult.Yes)
            {
                this.Close();
            }

        }


    }

}
